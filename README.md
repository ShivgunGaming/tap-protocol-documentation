# TAP Protocol
Documentation for TAP protocol - S.G

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

#### Examples

```javascript
{ 
  "p": "tap",
  "op": "token-deploy",
  "tick": "tap",
  "max": "21000000",
  "lim": "1000"
}
```

```javascript
{ 
  "p": "tap",
  "op": "token-mint",
  "tick": "tap",
  "amt": "1000"
}
```

```javascript
{ 
  "p": "tap",
  "op": "token-transfer",
  "tick": "tap",
  "amt": "100"
}
```

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

Mattyverse's Example!

```javascript
{
  "p" : "tap",
  "op" : "token-trade",
  "side" : "0",
  "tick" : "shiba",
  "amt" : "100000000",
  "accept" : [
    {
      "tick" : "based",
      "amt" : "1"
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

### `Fill A Trade`


To complete a trade that's valid and open for filling, you can use a "token-trade" function like this:

```javascript
{
  "p": "tap",
  "op": "token-trade",
  "side": "1",
  "trade": "<inscription id>",
  "tick": "based",
  "amt": "0.2",
  "fee_rcv": "<fee receiver address>"
}
```

Here's what each part means:

➼ Set "side" to 1 to indicate that you are the buyer in this trade.

➼ "trade" should match the unique ID of the original trade request you want to complete (not a number).

➼ "tick" and "amt" should match exactly one of the tokens listed in the original trade request.

➼ You must have the specified token and the exact amount, plus an additional 0.3% for trading fees if the "fee_rcv" attribute is set.

➼ If "fee_rcv" is set, it must be a valid Bitcoin address.

➼ If "fee_rcv" starts with "bc1," it must be trimmed and in lowercase.

➼ If the token in the "tick" attribute is a "cursed" one, you should also mark your trade as "cursed."

➼ If "cursed" support ends for new trades, the "tick" attribute must reference "cursed" tokens with a leading dash.

➼ After creating this trade request, you should send it to yourself for confirmation (tapping).

➼ If the current block upon tapping is later than the block specified in the "valid" attribute of the original trade request, the filling will fail.

➼ If all these conditions are met, the tokens and their exact amounts will be exchanged, and the trading fees will be paid to the fee receiver. Keep in mind that trading fees are only applied if they can be calculated as 0.3% of the transaction amount. If not, there may be no fees.

➼ For the trade to be considered successful, all the conditions should be met, and the balances of the parties involved should be adjusted correctly.

### `Token Auth`

The "token-auth" function in TAP is a way for third parties or authorities to create special signed instructions. These instructions can be used by anyone to send specific tokens to certain recipients. This system allows for custom rules about how tokens should be distributed, without TAP needing to understand all the details of these custom rules.

It's like TAP is a protocol that doesn't need to know the specifics of how the token distribution works because it relies on these signed instructions. Unlike some other systems, like zk-rollups, TAP's "token-auth" doesn't require special intermediaries or complex setups. It just uses signatures on messages.

You can even use "token-auth" to connect with zk-rollup systems, which adds an extra layer of security and reliability.

Importantly, "token-auth" doesn't rely on tricky technical tricks or data manipulation. All the operations are clearly defined and visible, making it easy for anyone to verify what's happening. This is important for transparency and trust.

This feature can be used for various purposes, like turning points into tokens in games, creating bridges between different token systems, managing staking and reward programs, and building cross-chain marketplaces, among other things.

### `Create A Authority`

To create an authority in TAP, which is a way for someone to have control over specific tokens, you need to follow these steps:

Send a special "token-auth" instruction and confirm it by sending it to yourself (tapping). This should be done from an address that already holds the tokens you want to authorize.

Manually creating the code for "token-auth" isn't recommended because it involves message signatures. Instead, you can use the provided example script to help generate these signed token authorizations. It uses a technology called secp256k1 signatures for this purpose.

Here's an example of what a "token-auth" instruction might look like:

```javascript
{
   "p":"tap",
   "op":"token-auth",
   "sig":{
      "v":"0",
      "r":"51143521410606380758535576062355234772504706283689533465002520447203156100709",
      "s":"23524754809729078525228087002160468580495709275023865450917881139756565577560"
   },
   "hash":"0f30c22be2f46e819538ca1281aadb82d3928cae5a699cade80013c5b14871e4",
   "salt":"0.25991027744263695",
   "auth":[
      "gib"
   ]
}
```

➼ The "auth" attribute should contain an array of specific TAP tokens that this authority should be in control of.

➼ If the "auth" array is empty, it means the authority will have control over all tokens owned by that account.

➼ Once the "token-auth" instruction is indexed, it will be verified against the signature ("sig"), the "hash" attribute, and the public key.

➼ The public key is retrieved by using the "hash."

➼ To ensure there are no collisions with other "hash" values, a custom "salt" value is provided by the authority.

➼ To verify, the "auth" array must be turned into a JSON string and hashed using SHA-256 along with the "salt" (sha256(auth + salt)).

➼ The "hash" must be unique and can only be used once in the entire TAP system.

➼ All tokens listed in "auth" must be deployed at the time of creating this "token-auth" (unless the "auth" array is empty).

➼ The authority address doesn't need to own the authorized tokens at the time of creating the "token-auth," but it's a good practice for the authority to own them when it's time to use them.

➼ If all authorized tokens are deployed, or if the "auth" array is empty, and the hashed signature is valid and the "hash" has never been used before, the "token-auth" instruction will be indexed after tapping it (sending it to "yourself").

### `Create A Redeem`

Creating a redeem means making certain tokens available for people to claim. Here's how it works:

Tokens that are ready to be claimed are authorized and signed by a trusted authority. This authority decides when and how these tokens can be claimed.

The process of creating a redeem is typically controlled by the authority, and they issue a special instruction for it.

It's not recommended for regular users to manually create these instructions because it requires complex message signatures. Instead, there are scripts available that can help generate these signed redeem instructions.

A typical redeem instruction includes details like:

A unique ID for the authority, indicating who's allowing the tokens to be claimed.
A list of tokens and the amounts that can be claimed.
The recipient's address where the claimed tokens should be sent.
The authority signs this redeem instruction using a cryptographic method called secp256k1 signatures to make sure it's legitimate.

This redeem instruction also contains a unique "hash" and a "salt" to ensure it's distinct and genuine, preventing any tampering or fraud.

Once this redeem instruction is created, anyone can use it to claim the tokens. There's no need for additional confirmation because the authority has already verified it.

To ensure everything is legitimate, the recipient's public key is checked to match the original authority's details.

The original "auth" instruction that initially authorized these tokens must remain valid and not be canceled.

All the tokens listed in the redeem instruction should also be present in the original "auth" instruction, ensuring everything is consistent.

If all these conditions are met, the tokens mentioned in the redeem instruction will be sent to the recipient's address just like TAP's "token-send" function. No additional confirmation (tapping) is required because this is a pre-approved and signed transaction that can be initiated by anyone who has the redeem instruction.

In simple terms, creating a redeem means that someone trusted has given the green light for you to claim certain tokens, and you can do so by following the provided instructions. It's a secure and transparent process.

```javascript
{
   "p":"tap",
   "op":"token-auth",
   "sig":{
      "v":"1",
      "r":"113472523327934685528808901641630457916054343054143422440331961430719594721038",
      "s":"21393407019197854961723689634443789868582208930187383447036700552814535514199"
   },
   "hash":"82e2b0d098dcdab820ff866b011250af8841a6b59cedd7164bb94b63d2598de9",
   "salt":"0.46074583388095514",
   "redeem":{
      "items":[
         {
            "tick":"gib",
            "amt":"546",
            "address":"bc1p9lpne8pnzq87dpygtqdd9vd3w28fknwwgv362xff9zv4ewxg6was504w20"
         }
      ],
      "auth":"fd3664a56cf6d14b21504e5d83a3d4867ee256f06cbe3bddf2787d6a80a86078i0",
      "data":""
   }
}
```



### `Cancel an authority`

To cancel a "token-auth", the authority must send an inscription like below to its associated address and tap.

{
   "p":"tap",
   "op":"token-auth",
   "cancel" : "fd3664a56cf6d14b21504e5d83a3d4867ee256f06cbe3bddf2787d6a80a86078i0"
}
"cancel" must point to an existing an non-cancelled "token-auth" of the authority.
Once tapped, no further redeems may be executed on the inscribed "token-auth", indefinitely.
