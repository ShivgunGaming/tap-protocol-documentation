# TAP Protocol
Documentation about TAP protocol

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
