# Examples of Smart Contract Implementation

## 1. Voting

The following contract is quite complex and it implements a commissioned voting contract. Of course, the main problems with electronic voting is how to assign voting rights to the right people and how to prevent them from being manipulated. We will not resolve all the problems here, but at least we will show how to conduct a proxy vote that is automatic and completely transparent at the same time.

Our idea is to create a contract for each (vote) vote, with an abbreviation for each option. Then, as the creator of the contract - the chairman, each independent address will be given the right to vote.

Address owners can choose to vote on their own, or delegate to someone they trust to vote.

At the end of the voting period, ```WinningProposal()```will return the proposal that receives the most votes.

This contract implementation uses two files, as follows:

Source File：```myballot_types.go``` 

```go
package myballot

import (
    "blockchain/smcsdk/sdk/types"
)

// Voter this declares a new complex type which will
//       be used for variables later.
//       it will represent a single voter.
type Voter struct {
    weight   uint          // weight is accumulated by delegation
    voted    bool          // if true, that person already voted
    delegate types.Address // person delegated to
    vote     uint          // index of the voted proposal
}

//Proposal this is a type for a single proposal.
type Proposal struct {
    name      string       // short name (up to 32 bytes)
    voteCount uint         // number of accumulated votes
}
```

Source File： ```myballot.go``` 

```go
package myballot

import (
    "blockchain/smcsdk/sdk"
    "blockchain/smcsdk/sdk/forx"
    "blockchain/smcsdk/sdk/types"
)

//Ballot a demo smart contract for voting with delegation.
//@:contract:myballot
//@:version:1.0
//@:organization:orgBtjfCSPCAJ84uQWcpNr74NLMWYm5SXzer
//@:author:b37e7627431feb18123b81bcf1f41ffd37efdb90513d48ff2c7f8a0c27a9d06c
type Ballot struct {
    sdk sdk.ISmartContract

    //chairperson this declares a state variable that stores a chairperson's 
    //      address for the contract
    //@:public:store:cache
    chairperson string

    //voters this declares a state variable that stores a 'Voter' struct for
    //    each possible address
    //@:public:store
    voters map[types.Address]Voter

    //proposals a dynamically-sized array of 'Proposal' structs
    //@:public:store:cache
    proposals []Proposal
}

//InitChain init when deployed on the blockChain first time
//@:constructor
func (ballot *Ballot) InitChain() {
}

//Init create a new (voting) vote for each proposal in 'proposal Names'
//@:public:method:gas[500]
func (ballot *Ballot) Init(proposalNames []string) {
    sender := ballot.sdk.Message().Sender().Address()

    // Only cntract's owner can perform init
    sdk.RequireOwner()

    proposals := ballot._proposals()
    sdk.Require(len(proposals) <= 0,
        types.ErrUserDefined, "Already inited")

    chairperson := sender
    ballot._setChairperson(chairperson)

    voter := ballot._voters(chairperson)
    voter.weight = 1
    ballot._setVoters(chairperson, voter)

    // For each of the provided proposal names,
    // create a new 'Proposal' object and add it to the end of the array
    forx.Range(proposalNames, func(i int, pName string) {
        proposals = append(proposals,
            Proposal{
            name:   pName,
            voteCount: 0,
        })
    })
    ballot._setProposals(proposals)
}

//GiveRightToVote give `voter` the right to vote on this ballot.
//                may only be called by 'chairperson'.
//@:public:method:gas[500]
func (ballot *Ballot) GiveRightToVote(voterAddr types.Address) {
    sender := ballot.sdk.Message().Sender().Address()
    chairperson := ballot._chairperson()
    sdk.Require(sender == chairperson,
        types.ErrNoAuthorization, "Only chairperson can give right to vote.")
    
    voter := ballot._voters(voterAddr)
    sdk.Require(voter.voted == false,
        types.ErrUserDefined, "The voter already voted.")
    sdk.Require(voter.weight == 0,
        types.ErrUserDefined, "The voter's weight must be zero.")

    voter.weight = 1
    ballot._setVoters(voterAddr, voter)
}

//Delegate Delegate your vote to the voter 'to'
//@:public:method:gas[1500]
func (ballot *Ballot) Delegate(to types.Address) {
    sender := ballot.sdk.Message().Sender().Address()
    sendVoter := ballot._voters(sender)

    sdk.Require(sendVoter.voted == false,
        types.ErrUserDefined, "You already voted.")
    sdk.Require(to != sender,
        types.ErrUserDefined, "Self-delegation is disallowed.")

    // Forward the delegation as long as 'to' also delegated.
    // In general, such loops are very dangerous, because if they run too 
    // long, they might need more gas than is available in a block.
    // In this case, the delegation will not be executed, but in other 
    // situations, such loops might cause a contract to get "stuck" completely.
    toVoter := ballot._voters(to)
    forx.Range( func() bool {
            return toVoter.delegate != ""
        },
        func(i int) {
            to = toVoter.delegate
            toVoter = ballot._voters(to)

            // We found a loop in the delegation, not allowed.
            sdk.Require(to != sender,
                types.ErrUserDefined, "Found loop in delegation.")
        })

    sendVoter.voted = true
    sendVoter.delegate = to
    delegate := toVoter
    if delegate.voted {
        // If the delegate already voted,
        // directly add to the number of votes
        proposals := ballot._proposals()
        proposals[int(delegate.vote)].voteCount += sendVoter.weight
        ballot._setProposals(proposals)
    } else {
        // If the delegate did not vote yet,
        // add to her weight.
        delegate.weight += sendVoter.weight
        ballot._setVoters(to, delegate)
    }
    return
}

//Vote give your vote (including votes delegated to you)
//     to proposal `proposals[proposal].name`.
//@:public:method:gas[500]
func (ballot *Ballot) Vote(proposal uint) {
    sender := ballot.sdk.Message().Sender().Address()
    sendVoter := ballot._voters(sender)

    sdk.Require(sendVoter.voted == false,
        types.ErrUserDefined, "You already voted.")

    proposals := ballot._proposals()
    sdk.Require(proposal < uint(len(proposals)),
        types.ErrUserDefined, "Proposal is out of index.")

    sendVoter.voted = true
    sendVoter.vote = proposal
    proposals[int(proposal)].voteCount += sendVoter.weight
    ballot._setProposals(proposals)
}

//WinningProposal computes the winning proposal taking all
//                previous votes into account.
//@:public:method:gas[500]
func (ballot *Ballot) WinningProposal() (winningProposal uint) {
    var winningVoteCount uint

    proposals := ballot._proposals()
    forx.Range(proposals, func(i int, proposal Proposal) {
        if proposal.voteCount > winningVoteCount {
            winningVoteCount = proposal.voteCount
            winningProposal = uint(i)
        }
    })
    return
}

//WinnerName calls winningProposal() function to get the index
//     of the winner contained in the proposals array and then
//     returns the name of the winner
//@:public:method:gas[500]
func (ballot *Ballot) WinnerName() (winnerName string) {
    proposals := ballot._proposals()
    if len(proposals) > 0 {
        winnerName = proposals[ballot.WinningProposal()].name
    }
    return
}
```

## 2. Charity Donation

The following contract implements a simple charitable donation function. The contract code are as follows:

```go
package mydonation

import (
    "blockchain/smcsdk/sdk"
    "blockchain/smcsdk/sdk/bn"
    "blockchain/smcsdk/sdk/forx"
    "blockchain/smcsdk/sdk/std"
    "blockchain/smcsdk/sdk/types"
)

//Mydonation This is struct of contract
//@:contract:mydonation
//@:version:1.0
//@:organization:orgBtjfCSPCAJ84uQWcpNr74NLMWYm5SXzer
//@:author:b37e7627431feb18123b81bcf1f41ffd37efdb90513d48ff2c7f8a0c27a9d06c
type Mydonation struct {
    sdk sdk.ISmartContract

    //Total donations received by donees
    //@:public:store
    donations map[types.Address]bn.Number // key=address of donee
}

const (
    errDoneeCannotBeOwner = 55000 + iota
    errDoneeCannotBeSmc
    errDoneeAlreadyExist
    errDoneeNotExist
    errDonationExist
    errDonationNotEnough
)

//@:public:receipt
type receipt interface {
    emitAddDonee(donee types.Address)
    emitDelDonee(donee types.Address)
    emitDonate(from, donee types.Address, value, balance bn.Number)
    emitTransferDonation(donee types.Address, value, balance bn.Number)
}

//InitChain Constructor of this Mydonation
//@:constructor
func (d *Mydonation) InitChain() {
}

//AddDonee Add a new donee
//@:public:method:gas[500]
func (d *Mydonation) AddDonee(donee types.Address) {
    sdk.RequireOwner()
    sdk.RequireAddress(donee)
    sdk.Require(donee != d.sdk.Message().Sender().Address(),
        errDoneeCannotBeOwner, "Donee can not be owner")
    sdk.Require(donee != d.sdk.Message().Contract().Address(),
        errDoneeCannotBeSmc, "Donee can not be this smart contract")
    sdk.Require(donee != d.sdk.Message().Contract().Account().Address(),
        errDoneeCannotBeSmc, "Donee can not be account of this smart contract")
    sdk.Require(!d._chkDonations(donee),
        errDoneeAlreadyExist, "Donee already exists")

    d._setDonations(donee, bn.N(0))

    //emit receipt
    d.emitAddDonee(donee)
}

//Donate delete a donee
//@:public:method:gas[500]
func (d *Mydonation) DelDonee(donee types.Address) {
    sdk.RequireOwner()
    sdk.RequireAddress(donee)
    sdk.Require(d._chkDonations(donee),
        errDoneeNotExist, "Donee does not exist")
    sdk.Require(d._donations(donee).IsEqualI(0),
        errDonationExist, "Donation exists")

    d._delDonations(donee)

    //emit receipt
    d.emitDelDonee(donee)
}

//Donate Charitable donors donate money to smart contract
//@:public:method:gas[500]
func (d *Mydonation) Donate(donee types.Address) {
    sdk.RequireAddress(donee)
    sdk.Require(d._chkDonations(donee),
        errDoneeNotExist, "Donee does not exist")

    var valTome *std.Transfer
    token := d.sdk.Helper().GenesisHelper().Token()
    forx.Range(
        d.sdk.Message().GetTransferToMe(), 
        func(i int, receipt *std.Transfer) {
            sdk.Require(receipt.Token == token.Address(),
                types.ErrInvalidParameter, 
                "Accept donations in genesis token only")
            sdk.Require(valTome == nil,
                types.ErrInvalidParameter, 
                "Accept only one donation at a time")
            valTome = receipt
        },
    )
    sdk.Require(valTome != nil,
        types.ErrInvalidParameter, "Please transfer token to me first")

    balance := d._donations(donee).Add(valTome.Value)
    d._setDonations(donee, balance)

    //emit receipt
    d.emitDonate(
        d.sdk.Message().Sender().Address(),
        donee,
        valTome.Value,
        balance,
    )
}

//Withdraw To transfer donations to donee
//@:public:method:gas[500]
func (d *Mydonation) Transfer(donee types.Address, value bn.Number) {
    sdk.RequireOwner()
    sdk.RequireAddress(donee)
    sdk.Require(d._chkDonations(donee),
        errDoneeNotExist, 
        "Donee does not exist")
    sdk.Require(value.IsGreaterThanI(0),
        types.ErrInvalidParameter, 
        "Parameter \"value\" must be greater than 0")
    sdk.Require(d._donations(donee).IsGE(value),
        errDonationNotEnough, 
        "Donation is not enough")

    token := d.sdk.Helper().GenesisHelper().Token()
    account := d.sdk.Message().Contract().Account()
    account.TransferByToken(token.Address(), donee, value)
    balance := d._donations(donee).Sub(value)
    d._setDonations(donee, balance)

    //emit receipt
    d.emitTransferDonation(
        donee,
        value,
        balance,
    )
}
```
