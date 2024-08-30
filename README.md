# Circles EthAccra 2024 workshop project
This project is meant as an introduction to the [Circles SDK](https://docs.aboutcircles.com).
There's a short video about the application on [YouTube](https://www.youtube.com/watch?v=EqowvBKE5EY). 

A full-featured client for the v2 contracts can be found here: https://lionfish-app-eypqs.ondigitalocean.app/
The code for that client can be found here: https://github.com/aboutcircles/5ecret-garden

It's split in two parts. The first part is a demo of the application, and the second part is a code tour:

* Demo:
  * [0:00](https://youtu.be/EqowvBKE5EY?feature=shared) Begin
    Outlining the setup (two MetaMask wallets simulating two Circles users)
  * [0:34](https://youtu.be/EqowvBKE5EY?feature=shared&t=34) Prompt to get invited to Circles 
  * [0:56](https://youtu.be/EqowvBKE5EY?feature=shared&t=56) Inviting a new user to Circles from [5ecret-garden](https://lionfish-app-eypqs.ondigitalocean.app/)
  * [1:11](https://youtu.be/EqowvBKE5EY?feature=shared&t=71) Receiving the invitation
  * [1:15](https://youtu.be/EqowvBKE5EY?feature=shared&t=75) Brief explanation of Circles profiles
  * [1:29](https://youtu.be/EqowvBKE5EY?feature=shared&t=89) Fund the invited EOA with xDai
  * [2:03](https://youtu.be/EqowvBKE5EY?feature=shared&t=123) Create a Circles profile
  * [2:36](https://youtu.be/EqowvBKE5EY?feature=shared&t=156) Dashboard with balances and trust list
  * [2:52](https://youtu.be/EqowvBKE5EY?feature=shared&t=172) Establish trust with the inviter account
  * [3:28](https://youtu.be/EqowvBKE5EY?feature=shared&t=208) What does 'trust' mean?
  * [3:45](https://youtu.be/EqowvBKE5EY?feature=shared&t=225) Where do Circles come from?
  * [4:17](https://youtu.be/EqowvBKE5EY?feature=shared&t=257) Send the invited account some Circles from  [5ecret-garden](https://lionfish-app-eypqs.ondigitalocean.app/)
  * [4:58](https://youtu.be/EqowvBKE5EY?feature=shared&t=298) Mint new personal Circles
  * [5:38](https://youtu.be/EqowvBKE5EY?feature=shared&t=338) Send Circles to the inviter from the example app (Approval & Transfer)
  * [6:49](https://youtu.be/EqowvBKE5EY?feature=shared&t=409) Send Circles to the inviter from the example app (Transfer)

* Code:
  * [7:32](https://youtu.be/EqowvBKE5EY?feature=shared&t=452) Beginning of the code tour
  * [8:22](https://youtu.be/EqowvBKE5EY?feature=shared&t=502) Circles config
  * [8:34](https://youtu.be/EqowvBKE5EY?feature=shared&t=502) Routing in SvelteKit
  * [8:49](https://youtu.be/EqowvBKE5EY?feature=shared&t=529) The index page
  * [8:58](https://youtu.be/EqowvBKE5EY?feature=shared&t=538) Circles SDK initialization
  * [10:11](https://youtu.be/EqowvBKE5EY?feature=shared&t=611) Using getAvatarInfo() to check if the account already exists
  * [10:19](https://youtu.be/EqowvBKE5EY?feature=shared&t=619) Subscribing to events
  * [10:40](https://youtu.be/EqowvBKE5EY?feature=shared&t=640) Dashboard
  * [11:06](https://youtu.be/EqowvBKE5EY?feature=shared&t=666) Actions
    * [11:12](https://youtu.be/EqowvBKE5EY?feature=shared&t=672) Invite
    * [11:23](https://youtu.be/EqowvBKE5EY?feature=shared&t=683) Mint personal Circles
    * [11:23](https://youtu.be/EqowvBKE5EY?feature=shared&t=683) Mint personal Circles
    * [11:32](https://youtu.be/EqowvBKE5EY?feature=shared&t=692) Send Circles
    * [12:21](https://youtu.be/EqowvBKE5EY?feature=shared) Trust
    * [12:37](https://youtu.be/EqowvBKE5EY?feature=shared&t=757) Profile