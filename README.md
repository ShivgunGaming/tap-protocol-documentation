# TAP Protocol Documentation
Documentation about TAP protocol

![tap](https://github.com/ShivgunGaming/tap-protocol/assets/102505925/5ff24f19-05db-4138-b36d-9714622c5d67)

# Intro

TAP is a system that makes it easy for people to use and create crypto. It's designed to be simple and doesn't rely on complex technology or L2s. Instead, it uses a straightforward method called tapping to make sure transactions are secure.

The main idea behind TAP is to build on the success of a previous system called BRC-20, but TAP wants to stay independent and not be controlled by a central authority. People from the community can suggest and add new features to TAP through a system of community-based decision-making.

There are some special codes called tickers, and some of them (those with lengths of 1, 2, and 4) are set aside for connecting BRC-20 tokens with TAP tokens in the future, but you can't use them on TAP right now.

# The functionality of TAP

TAP has two parts, external and internal.

### `External`
This part works just like a system called BRC-20. To connect to TAP, online marketplaces and digital wallets need to copy their existing BRC-20 technology. Then, they either have to add TAP's special features or use connections that have these features to check how much of a token you have and if you can send it to someone else. Once this is set up, you can trade TAP tokens just like you do with regular BRC-20 tokens on these platforms.

### `Internal:` 
This part is for users and has some cool features. You can "stake" your tokens, which means you lock them up for a while to earn rewards. You can also easily swap tokens for other tokens. There's even a way to send lots of tokens to many people at once. The TAP community, using a special token called $TRAC (which is a type of BRC-20 token), decides which new features to add or improve.

Because both BRC-20 and TAP tokens don't have a way to tell if a token is "cursed" or not, some systems need to check for negative numbers to separate cursed tokens from normal ones. 

# External:

Before a certain date when they stop supporting "cursed" tokens, you can't use a dash at the beginning of the code for any function in their system. But once they stop supporting these cursed tokens, you can use dashes at the start of the codes for functions.

If the system you're using can't handle cursed tokens, they need to add a check to make sure tokens that come from these cursed tokens (ones with a code that starts with a dash) are not included in their system. 

# Internal:

### `Token Send`

The "token-send" is a new feature that allows you to send lots of different tokens to many people all at once. Here's how it works:

You start by telling your Bitcoin address that you want to use "token-send" to send tokens. After your Bitcoin transaction is confirmed, you have to tell your address again to make sure everything is right (this is called "tapping").

Your Bitcoin address must have enough tokens for the amounts you want to send. Only if you "tap" the "token-send" feature, it will actually send the tokens. The addresses of the people who will get the tokens need to be real Bitcoin addresses, without extra spaces, and those that start with "bc1" need to be in lowercase.

When you first set up "token-send," everything you write, like amounts and addresses, must be correct. When you "tap" it, the system will check that the sends are valid and make sure there are enough tokens and the data is right.

Every successful send will take the tokens from your balance and give them to the recipients.

"Token-send" only works with the available balance, not the total balance (available balance = total balance - transferable).

There's no difference between "cursed" and "non-cursed" tokens in the "items" attribute. If a token is "cursed," you have to use a dash in front of its code to work with it, unlike how you use "token-send" 
functions.

This is so you can mix "cursed" and "non-cursed" tokens in the same transaction.

#### Example

The below example will send 1000 tap tokens to each address. Tokens and amounts can be mixed.
There is no restriction on the amount of items (receivers).

```javascript
{
  "p" : "tap",
  "op" : "token-send",
  "items" : [
     {
      "tick": "-tap",
      "amt": "10000",
      "address" : "bc1p9lpne8pnzq87dpygtqdd9vd3w28fknwwgv362xff9zv4ewxg6was504w20"
     },
     {
      "tick": "tap",
      "amt": "10000",
      "address" : "bc1p063utyzvjuhkn0g06l5xq6e9nv6p4kjh5yxrwsr94de5zfhrj7csns0aj4"
     }
  ]
}
```

### `Token Trade`

The "token-trade" function is a set of tools for trading tokens using written instructions. Here's how it works:

If you want to sell tokens, you create a trade request by writing down the details of what you're selling. This trade request is then stored in a collection, and it can be seen by anyone looking to buy tokens. This request is available for a certain number of blocks.

When someone wants to buy the tokens you're offering, they can fulfill your trade request. If they do, an optional fee receiver can be added. This person will get a 0.3% fee in tokens for each trade that gets completed. The buyer pays this fee in addition to the cost of the tokens they're buying. These fees help the people or services that manage the trade requests and the marketplace to make some money. Buyers find this system convenient because it guides them through the process, and it benefits the services involved.

To inscribe a trade, a seller has to inscribe a text inscription in the following format:

### `Creating a Trade`

```javascript
{
  "p" : "tap",
  "op" : "token-trade",
  "side" : "0",
  "tick" : "tap",
  "amt" : "1",
  "accept" : [
    {
      "tick" : "buidl",
      "amt" : "0.1"
    },
    {
      "tick" : "based",
      "amt" : "0.2"
    }
  ],
  "valid" : "900000"
}
```

When someone creates a trade request, it becomes active and can be fulfilled under these conditions:

➼ All the information in the trade request is necessary.

➼ The "Side" attribute set to 0 means it's a trade request made by a seller.

➼ The person selling must have the tokens they're offering at the time the trade is completed by a buyer.

➼ The number of tokens they're offering should be taken from the available balance, not the total balance (available balance = total balance - tokens that are already being transferred).

➼ The tokens listed in the "accept" attribute are the ones the seller is willing to receive in exchange for the trade.

➼ If the buyer uses any of these accepted tokens, the trade is considered complete.

➼ The tokens listed in the "accept" attribute must be available at the time the trade is fulfilled by a buyer.

➼ The "valid" attribute block must be in the future or at the current block (inclusive). If the current block is past the "valid" block, the trade is invalid and won't be listed.

➼ If the tokens being offered are "cursed," the trade request must also be marked as "cursed."

➼ The tickers within the "accept" attribute must be unique. If there are multiple instances of the same ticker, only the first one will be listed.

➼ If any "cursed" tokens are listed in the "accept" attribute, they must have a dash "-" in front of the ticker. This is to allow trading of both "cursed" and non-"cursed" tokens in the same trade.

➼ After creating the trade request, the sellers need to send the request to themselves to confirm it (tapping).

### `Cancel A Trade`

To cancel a trade that's still active and hasn't expired, you need to do the following:

Create and tap (confirm) a special function like this:

```javascript
{
  "p": "tap",
  "op": "token-trade",
  "side": "0",
  "trade": "<inscription id>"
}
```

➼ You must fill in all the details as shown in the example above.

➼ Setting "side" to 0 indicates that this is a trade cancellation request made by the seller.

➼ The "trade" attribute should contain the unique inscription ID (not a number) of the trade you want to cancel.

➼ The trade will only be canceled if the trade you're referring to is owned by the same wallet address that's trying to cancel it.

➼ Once a trade has been successfully canceled, no more trades can happen related to that trade request.

➼ After you've created this cancellation request, you should send it to yourself for confirmation (tapping).
