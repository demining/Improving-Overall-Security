

# Improving the overall security of the ecosystem from attacks on smart contracts

---


---






<figure class="aligncenter size-large"><img decoding="async" width="1024" height="576" src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/035-1024x576.png" alt="Improving the overall security of the ecosystem from attacks on smart contracts" class="wp-image-1996" srcset="https://cryptodeeptech.ru/wp-content/uploads/2023/03/035-1024x576.png 1024w, https://cryptodeeptech.ru/wp-content/uploads/2023/03/035-300x169.png 300w, https://cryptodeeptech.ru/wp-content/uploads/2023/03/035-768x432.png 768w, https://cryptodeeptech.ru/wp-content/uploads/2023/03/035.png 1280w" sizes="(max-width: 1024px) 100vw, 1024px"></figure></div>


---


* Tutorial: https://youtu.be/HVh_cbsgSMg
* Tutorial: https://cryptodeeptech.ru/improving-overall-security


---





<hr class="wp-block-separator has-alpha-channel-opacity">



<p></p>



<h2>Front-Running AKA Transaction-Ordering Dependence</h2>



<p>The University of Concordia considers front-running to be, “a course of action where an entity benefits from prior access to privileged market information about upcoming transactions and trades.” This knowledge of future events in a market can lead to exploitation.</p>



<p>For example, knowing that a very large purchase of a specific token is going to occur, a bad actor can purchase that token in advance, and sell the token for a profit when the oversized buy order increases the price.</p>



<p><a href="https://cryptodeeptech.ru/blockchain-attack-vectors/" target="_blank" rel="noreferrer noopener">Front-running attacks have long been an issue in financial markets, and due to blockchain’s transparent nature, the problem is coming up again in cryptocurrency markets.</a></p>



<p>Since the solution to this problem varies on a per contract basis, it can be hard to protect against. Possible solutions include batching transactions and using a pre-commit scheme, i.e. allow users to submit details at a later time.</p>



<p class="has-text-align-center has-medium-font-size"><a href="https://cryptodeep.ru/doc/SoK_Transparent_Dishonesty_Front-Running_Attacks_on_Blockchain.pdf" target="_blank" rel="noreferrer noopener"><strong>PDF:  SoK: Transparent Dishonesty: Front-Running Attacks on Blockchain</strong></a></p>



<h1>Frontrunning</h1>



<p>Since all transactions are visible in the mempool for a short while before being executed, observers of the network can see and react to an action before it is included in a block. An example of how this can be exploited is with a decentralized exchange where a buy order transaction can be seen, and second order can be broadcast and executed before the first transaction is included. Protecting against this is difficult, as it would come down to the specific contract itself.</p>



<p>Front-running, coined originally for traditional financial markets, is the race to order the chaos to the winner’s benefit. In financial markets, the flow of information gave birth to intermediaries that could simply profit by being the first to know and react to some information. These attacks mostly had been within stock market deals and early domain registries, such as whois gateways.</p>



<p>front-run·ning (/ˌfrəntˈrəniNG/)</p>



<p><em>noun</em>: front-running;</p>



<ol>
<li><em>STOCK MARKET</em>the practice by market makers of dealing on advance information provided by their brokers and investment analysts, before their clients have been given the information.</li>
</ol>



<h3 id="taxonomy">Taxonomy</h3>



<p>By defining a&nbsp;<a href="https://arxiv.org/abs/1902.05164">taxonomy</a>&nbsp;and differentiating each group from another, we can make it easier to discuss the problem and find solutions for each group.</p>



<p><a href="https://cryptodeeptech.ru/blockchain-attack-vectors/" target="_blank" rel="noreferrer noopener">We define the following categories of front-running attacks</a>:</p>



<ol>
<li>Displacement</li>



<li>Insertion</li>



<li>Suppression</li>
</ol>



<h4 id="displacement">Displacement</h4>



<p>In the first type of attack,&nbsp;<em>a displacement attack</em>, it is&nbsp;<strong>not important</strong>&nbsp;for Alice’s (User) function call to run after Mallory (Adversary) runs her function. Alice’s can be orphaned or run with no meaningful effect. Examples of displacement include:</p>



<ul>
<li>Alice trying to register a domain name and Mallory registering it first;</li>



<li>Alice trying to submit a bug to receive a bounty and Mallory stealing it and submitting it first;</li>



<li>Alice trying to submit a bid in an auction and Mallory copying it.</li>
</ul>



<p>This attack is commonly performed by increasing the&nbsp;<code>gasPrice</code>&nbsp;higher than network average, often by a multiplier of 10 or more.</p>



<h4 id="insertion">Insertion</h4>



<p>For this type of attack, it is&nbsp;<strong>important</strong>&nbsp;to the adversary that the original function call runs after her transaction. In an insertion attack, after Mallory runs her function, the state of the contract is changed and she needs Alice’s original function to run on this modified state. For example, if Alice places a purchase order on a blockchain asset at a higher price than the best offer, Mallory will insert two transactions: she will purchase at the best offer price and then offer the same asset for sale at Alice’s slightly higher purchase price. If Alice’s transaction is then run after, Mallory will profit on the price difference without having to hold the asset.</p>



<p>As with displacement attacks, this is usually done by outbidding Alice’s transaction in the gas price auction.</p>



<p>Transaction Order Dependence</p>



<p>Transaction Order Dependence is equivalent to race condition in smart contracts. An example, if one function sets the reward percentage, and the withdraw function uses that percentage; then withdraw transaction can be front-run by a change reward function call, which impacts the amount that will be withdrawn eventually.</p>



<p>See&nbsp;<a href="https://swcregistry.io/docs/SWC-114">SWC-114</a></p>



<h4 id="suppression">Suppression</h4>



<p>In a suppression attack, a.k.a&nbsp;<em>Block Stuffing</em>&nbsp;attacks, after Mallory runs her function, she tries to delay Alice from running her function.</p>



<p>This was the case with the first winner of the “Fomo3d” game and some other on-chain hacks. The attacker sent multiple transactions with a high&nbsp;<code>gasPrice</code>&nbsp;and&nbsp;<code>gasLimit</code>&nbsp;to custom smart contracts that assert (or use other means) to consume all the gas and fill up the block’s&nbsp;<code>gasLimit</code>.</p>



<p>Variants</p>



<p><a href="https://cryptodeeptech.ru/blockchain-attack-vectors/" target="_blank" rel="noreferrer noopener">Each of these attacks has two variants,&nbsp;<em>asymmetric</em>&nbsp;and&nbsp;<em>bulk</em>.</a></p>



<p>In some cases, Alice and Mallory are performing different operations. For example, Alice is trying to cancel an offer, and Mallory is trying to fulfill it first. We call this&nbsp;<em>asymmetric displacement</em>. In other cases, Mallory is trying to run a large set of functions: for example, Alice and others are trying to buy a limited set of shares offered by a firm on a blockchain. We call this&nbsp;<em>bulk displacement</em>.</p>



<h3 id="mitigations">Mitigations</h3>



<p>Front-running is a pervasive issue on public blockchains such as Ethereum.</p>



<p>The best remediation is to&nbsp;<strong>remove the benefit of front-running in your application</strong>, mainly by removing the importance of transaction ordering or time. For example, in markets, it would be better to implement batch auctions (this also protects against high-frequency trading concerns). Another way is to use a pre-commit scheme (“I’m going to submit the details later”). A third option is to mitigate the cost of front-running by specifying a maximum or minimum acceptable price range on a trade, thereby limiting price slippage.</p>



<p><strong>Transaction Ordering:</strong>&nbsp;Go-Ethereum (Geth) nodes order the transactions based on their&nbsp;<code>gasPrice</code>&nbsp;and address nonce. This, however, results in a gas auction between participants in the network to get included in the block currently being mined.</p>



<p><strong>Confidentiality:</strong>&nbsp;Another approach is to limit the visibility of the transactions, this can be done using a “commit and reveal” scheme.</p>



<p>A simple implementation is to store the keccak256 hash of the data in the first transaction, then reveal the data and verify it against the hash in the second transaction. However note that the transaction itself leaks the intention and possibly the value of the collateralization. There are enhanced commit and reveal schemes that are more secure, however require more transactions to function, e.g.</p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h2>DoS with Block Gas Limit</h2>



<p>In the Ethereum blockchain, the blocks all have a gas limit. One of the benefits of a block gas limit is that it prevents attackers from creating an infinite transaction loop, but if the gas usage of a transaction exceeds this limit, the transaction will fail. This can lead to a DoS attack in a couple different ways.</p>



<h3><a href="https://github.com/allpaca/smart-contract-attack-vectors/blob/master/attacks/dos-gas-limit.md#unbounded-operations"></a>Unbounded Operations</h3>



<p>A situation in which the block gas limit can be an issue is in sending funds to an array of addresses. Even without any malicious intent, this can easily go wrong. Just by having too large an array of users to pay can max out the gas limit and prevent the transaction from ever succeeding.</p>



<p>This situation can also lead to an attack. Say a bad actor decides to create a significant amount of addresses, with each address being paid a small amount of funds from the smart contract. If done effectively, the transaction can be blocked indefinitely, possibly even preventing further transactions from going through.</p>



<p>An effective solution to this problem would be to use a pull payment system over the current push payment system. To do this, separate each payment into it’s own transaction, and have the recipient call the function.</p>



<p>If, for some reason, you really need to loop through an array of unspecified length, at least expect it to potentially take multiple blocks, and allow it to be performed in multiple transactions – as seen in this example:</p>



<pre class="wp-block-code"><code>struct Payee {
    address addr;
    uint256 value;
}

Payee[] payees;
uint256 nextPayeeIndex;

function payOut() {
    uint256 i = nextPayeeIndex;
    while (i &lt; payees.length &amp;&amp; msg.gas &gt; 200000) {
      payees[i].addr.send(payees[i].value);
      i++;
    }
    nextPayeeIndex = i;
}
</code></pre>



<h3><a href="https://github.com/allpaca/smart-contract-attack-vectors/blob/master/attacks/dos-gas-limit.md#block-stuffing"></a>Block Stuffing</h3>



<p>In some situations, your contract can be attacked with a block gas limit even if you don’t loop through an array of unspecified length. An attacker can fill several blocks before a transaction can be processed by using a sufficiently high gas price.</p>



<p>This attack is done by issuing several transactions at a very high gas price. If the gas price is high enough, and the transactions consume enough gas, they can fill entire blocks and prevent other transactions from being processed.</p>



<p>Ethereum transactions require the sender to pay gas to disincentivize spam attacks, but in some situations, there can be enough incentive to go through with such an attack. For example, a block stuffing attack was used on a gambling Dapp, Fomo3D. The app had a countdown timer, and users could win a jackpot by being the last to purchase a key, except everytime a user bought a key, the timer would be extended. An attacker bought a key then stuffed the next 13 blocks and a row so they could win the jackpot.</p>



<p>T<a href="https://cryptodeeptech.ru/blockchain-attack-vectors/" target="_blank" rel="noreferrer noopener">o prevent such attacks from occuring, it’s important to carefully consider whether it’s safe to incorporate time-based actions in your application.</a></p>



<h1>Denial of Service</h1>



<h2 id="dos-with-unexpected-revert">DoS with (Unexpected) revert</h2>



<p>Consider a simple auction contract:</p>



<pre id="__code_1" class="wp-block-code"><code>// INSECURE
contract Auction {
    address currentLeader;
    uint highestBid;

    function bid() payable {
        require(msg.value &gt; highestBid);

        require(currentLeader.send(highestBid)); // Refund the old leader, if it fails then revert

        currentLeader = msg.sender;
        highestBid = msg.value;
    }
}
</code></pre>



<p>If attacker bids using a smart contract which has a fallback function that reverts any payment, the attacker can win any auction. When it tries to refund the old leader, it reverts if the refund fails. This means that a malicious bidder can become the leader while making sure that any refunds to their address will&nbsp;<em>always</em>&nbsp;fail. In this way, they can prevent anyone else from calling the&nbsp;<code>bid()</code>&nbsp;function, and stay the leader forever. A recommendation is to set up a&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/development-recommendations/general/external-calls/">pull payment system</a>&nbsp;instead, as described earlier.</p>



<p>Another example is when a contract may iterate through an array to pay users (e.g., supporters in a crowdfunding contract). It’s common to want to make sure that each payment succeeds. If not, one should revert. The issue is that if one call fails, you are reverting the whole payout system, meaning the loop will never complete. No one gets paid because one address is forcing an error.</p>



<pre id="__code_2" class="wp-block-code"><code>address[] private refundAddresses;
mapping (address =&gt; uint) public refunds;

// bad
function refundAll() public {
    for(uint x; x &lt; refundAddresses.length; x++) { // arbitrary length iteration based on how many addresses participated
        require(refundAddresses[x].send(refunds[refundAddresses[x]])) // doubly bad, now a single failure on send will hold up all funds
    }
}
</code></pre>



<p>Again, the recommended solution is to&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/development-recommendations/general/external-calls/">favor pull over push payments</a>.</p>



<p>See&nbsp;<a href="https://swcregistry.io/docs/SWC-113">SWC-113</a></p>



<h2 id="dos-with-block-gas-limit">DoS with Block Gas Limit</h2>



<p>Each block has an upper bound on the amount of gas that can be spent, and thus the amount computation that can be done. This is the Block Gas Limit. If the gas spent exceeds this limit, the transaction will fail. This leads to a couple of possible Denial of Service vectors:</p>



<h3 id="gas-limit-dos-on-a-contract-via-unbounded-operations">Gas Limit DoS on a Contract via Unbounded Operations</h3>



<p>You may have noticed another problem with the previous example: by paying out to everyone at once, you risk running into the block gas limit.</p>



<p>This can lead to problems even in the absence of an intentional attack. However, it’s especially bad if an attacker can manipulate the amount of gas needed. In the case of the previous example, the attacker could add a bunch of addresses, each of which needs to get a very small refund. The gas cost of refunding each of the attacker’s addresses could, therefore, end up being more than the gas limit, blocking the refund transaction from happening at all.</p>



<p>This is another reason to&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/development-recommendations/general/external-calls/">favor pull over push payments</a>.</p>



<p>If you absolutely must loop over an array of unknown size, then you should plan for it to potentially take multiple blocks, and therefore require multiple transactions. You will need to keep track of how far you’ve gone, and be able to resume from that point, as in the following example:</p>



<pre id="__code_3" class="wp-block-code"><code>struct Payee {
    address addr;
    uint256 value;
}

Payee[] payees;
uint256 nextPayeeIndex;

function payOut() {
    uint256 i = nextPayeeIndex;
    while (i &lt; payees.length &amp;&amp; gasleft() &gt; 200000) {
      payees[i].addr.send(payees[i].value);
      i++;
    }
    nextPayeeIndex = i;
}
</code></pre>



<p>You will need to make sure that nothing bad will happen if other transactions are processed while waiting for the next iteration of the&nbsp;<code>payOut()</code>&nbsp;function. So only use this pattern if absolutely necessary.</p>



<h3 id="gas-limit-dos-on-the-network-via-block-stuffing">Gas Limit DoS on the Network via Block Stuffing</h3>



<p>Even if your contract does not contain an unbounded loop, an attacker can prevent other transactions from being included in the blockchain for several blocks by placing computationally intensive transactions with a high enough gas price.</p>



<p>To do this, the attacker can issue several transactions which will consume the entire gas limit, with a high enough gas price to be included as soon as the next block is mined. No gas price can guarantee inclusion in the block, but the higher the price is, the higher is the chance.</p>



<p>If the attack succeeds, no other transactions will be included in the block. Sometimes, an attacker’s goal is to block transactions to a specific contract prior to specific time.</p>



<p>This attack&nbsp;<a href="https://solmaz.io/2018/10/18/anatomy-block-stuffing/">was conducted</a>&nbsp;on Fomo3D, a gambling app. The app was designed to reward the last address that purchased a “key”. Each key purchase extended the timer, and the game ended once the timer went to 0. The attacker bought a key and then stuffed 13 blocks in a row until the timer was triggered and the payout was released. Transactions sent by attacker took 7.9 million gas on each block, so the gas limit allowed a few small “send” transactions (which take 21,000 gas each), but disallowed any calls to the&nbsp;<code>buyKey()</code>&nbsp;function (which costs 300,000+ gas).</p>



<p>A Block Stuffing attack can be used on any contract requiring an action within a certain time period. However, as with any attack, it is only profitable when the expected reward exceeds its cost. The cost of this attack is directly proportional to the number of blocks which need to be stuffed. If a large payout can be obtained by preventing actions from other participants, your contract will likely be targeted by such an attack.</p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h2>DoS with (Unexpected) revert</h2>



<p>DoS (Denial of Service) attacks can occur in functions when you try to send funds to a user and the functionality relies on that fund transfer being successful.</p>



<p>This can be problematic in the case that the funds are sent to a smart contract created by a bad actor, since they can simply create a fallback function that reverts all payments.</p>



<p>For example:</p>



<pre class="wp-block-code"><code>// INSECURE
contract Auction {
    address currentLeader;
    uint highestBid;

    function bid() payable {
        require(msg.value &gt; highestBid);

        require(currentLeader.send(highestBid)); // Refund the old leader, if it fails then revert

        currentLeader = msg.sender;
        highestBid = msg.value;
    }
}
</code></pre>



<p>As you can see in this example, if an attacker bids from a smart contract with a fallback function reverting all payments, they can never be refunded, and thus no one can ever make a higher bid.</p>



<p>This can also be problematic without an attacker present. For example, you may want to pay an array of users by iterating through the array, and of course you would want to make sure each user is properly paid. The problem here is that if one payment fails, the funtion is reverted and no one is paid.</p>



<pre class="wp-block-code"><code>address[] private refundAddresses;
mapping (address =&gt; uint) public refunds;

// bad
function refundAll() public {
    for(uint x; x &lt; refundAddresses.length; x++) { // arbitrary length iteration based on how many addresses participated
        require(refundAddresses[x].send(refunds[refundAddresses[x]])) // doubly bad, now a single failure on send will hold up all funds
    }
}
</code></pre>



<p>An effective solution to this problem would be to use a pull payment system over the current push payment system. To do this, separate each payment into it’s own transaction, and have the recipient call the function.</p>



<pre class="wp-block-code"><code>contract auction {
    address highestBidder;
    uint highestBid;
    mapping(address =&gt; uint) refunds;

    function bid() payable external {
        require(msg.value &gt;= highestBid);

        if (highestBidder != address(0)) {
            refunds[highestBidder] += highestBid; // record the refund that this user can claim
        }

        highestBidder = msg.sender;
        highestBid = msg.value;
    }

    function withdrawRefund() external {
        uint refund = refunds[msg.sender];
        refunds[msg.sender] = 0;
        (bool success, ) = msg.sender.call.value(refund)("");
        require(success);
    }
}
</code></pre>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h1>Denial of Service</h1>



<h2 id="dos-with-unexpected-revert">DoS with (Unexpected) revert</h2>



<p>Consider a simple auction contract:</p>



<pre id="__code_1" class="wp-block-code"><code>// INSECURE
contract Auction {
    address currentLeader;
    uint highestBid;

    function bid() payable {
        require(msg.value &gt; highestBid);

        require(currentLeader.send(highestBid)); // Refund the old leader, if it fails then revert

        currentLeader = msg.sender;
        highestBid = msg.value;
    }
}
</code></pre>



<p>If attacker bids using a smart contract which has a fallback function that reverts any payment, the attacker can win any auction. When it tries to refund the old leader, it reverts if the refund fails. This means that a malicious bidder can become the leader while making sure that any refunds to their address will&nbsp;<em>always</em>&nbsp;fail. In this way, they can prevent anyone else from calling the&nbsp;<code>bid()</code>&nbsp;function, and stay the leader forever. A recommendation is to set up a&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/development-recommendations/general/external-calls/">pull payment system</a>&nbsp;instead, as described earlier.</p>



<p>Another example is when a contract may iterate through an array to pay users (e.g., supporters in a crowdfunding contract). It’s common to want to make sure that each payment succeeds. If not, one should revert. The issue is that if one call fails, you are reverting the whole payout system, meaning the loop will never complete. No one gets paid because one address is forcing an error.</p>



<pre id="__code_2" class="wp-block-code"><code>address[] private refundAddresses;
mapping (address =&gt; uint) public refunds;

// bad
function refundAll() public {
    for(uint x; x &lt; refundAddresses.length; x++) { // arbitrary length iteration based on how many addresses participated
        require(refundAddresses[x].send(refunds[refundAddresses[x]])) // doubly bad, now a single failure on send will hold up all funds
    }
}
</code></pre>



<p>Again, the recommended solution is to&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/development-recommendations/general/external-calls/">favor pull over push payments</a>.</p>



<p>See&nbsp;<a href="https://swcregistry.io/docs/SWC-113">SWC-113</a></p>



<h2 id="dos-with-block-gas-limit">DoS with Block Gas Limit</h2>



<p>Each block has an upper bound on the amount of gas that can be spent, and thus the amount computation that can be done. This is the Block Gas Limit. If the gas spent exceeds this limit, the transaction will fail. This leads to a couple of possible Denial of Service vectors:</p>



<h3 id="gas-limit-dos-on-a-contract-via-unbounded-operations">Gas Limit DoS on a Contract via Unbounded Operations</h3>



<p>You may have noticed another problem with the previous example: by paying out to everyone at once, you risk running into the block gas limit.</p>



<p>This can lead to problems even in the absence of an intentional attack. However, it’s especially bad if an attacker can manipulate the amount of gas needed. In the case of the previous example, the attacker could add a bunch of addresses, each of which needs to get a very small refund. The gas cost of refunding each of the attacker’s addresses could, therefore, end up being more than the gas limit, blocking the refund transaction from happening at all.</p>



<p>This is another reason to&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/development-recommendations/general/external-calls/">favor pull over push payments</a>.</p>



<p>If you absolutely must loop over an array of unknown size, then you should plan for it to potentially take multiple blocks, and therefore require multiple transactions. You will need to keep track of how far you’ve gone, and be able to resume from that point, as in the following example:</p>



<pre id="__code_3" class="wp-block-code"><code>struct Payee {
    address addr;
    uint256 value;
}

Payee[] payees;
uint256 nextPayeeIndex;

function payOut() {
    uint256 i = nextPayeeIndex;
    while (i &lt; payees.length &amp;&amp; gasleft() &gt; 200000) {
      payees[i].addr.send(payees[i].value);
      i++;
    }
    nextPayeeIndex = i;
}
</code></pre>



<p>You will need to make sure that nothing bad will happen if other transactions are processed while waiting for the next iteration of the&nbsp;<code>payOut()</code>&nbsp;function. So only use this pattern if absolutely necessary.</p>



<h3 id="gas-limit-dos-on-the-network-via-block-stuffing">Gas Limit DoS on the Network via Block Stuffing</h3>



<p>Even if your contract does not contain an unbounded loop, an attacker can prevent other transactions from being included in the blockchain for several blocks by placing computationally intensive transactions with a high enough gas price.</p>



<p>To do this, the attacker can issue several transactions which will consume the entire gas limit, with a high enough gas price to be included as soon as the next block is mined. No gas price can guarantee inclusion in the block, but the higher the price is, the higher is the chance.</p>



<p>If the attack succeeds, no other transactions will be included in the block. Sometimes, an attacker’s goal is to block transactions to a specific contract prior to specific time.</p>



<p>This attack&nbsp;<a href="https://solmaz.io/2018/10/18/anatomy-block-stuffing/">was conducted</a>&nbsp;on Fomo3D, a gambling app. The app was designed to reward the last address that purchased a “key”. Each key purchase extended the timer, and the game ended once the timer went to 0. The attacker bought a key and then stuffed 13 blocks in a row until the timer was triggered and the payout was released. Transactions sent by attacker took 7.9 million gas on each block, so the gas limit allowed a few small “send” transactions (which take 21,000 gas each), but disallowed any calls to the&nbsp;<code>buyKey()</code>&nbsp;function (which costs 300,000+ gas).</p>



<p>A Block Stuffing attack can be used on any contract requiring an action within a certain time period. However, as with any attack, it is only profitable when the expected reward exceeds its cost. The cost of this attack is directly proportional to the number of blocks which need to be stuffed. If a large payout can be obtained by preventing actions from other participants, your contract will likely be targeted by such an attack.</p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h1>External Calls</h1>



<h4 id="use-caution-when-making-external-calls">Use caution when making external calls</h4>



<p>Calls to untrusted contracts can introduce several unexpected risks or errors. External calls may execute malicious code in that contract&nbsp;<em>or</em>&nbsp;any other contract that it depends upon. As such, every external call should be treated as a potential security risk. When it is not possible, or undesirable to remove external calls, use the recommendations in the rest of this section to minimize the danger.</p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h4 id="mark-untrusted-contracts">Mark untrusted contracts</h4>



<p>When interacting with external contracts, name your variables, methods, and contract interfaces in a way that makes it clear that interacting with them is potentially unsafe. This applies to your own functions that call external contracts.</p>



<pre id="__code_1" class="wp-block-code"><code>// bad
Bank.withdraw(100); // Unclear whether trusted or untrusted

function makeWithdrawal(uint amount) { // Isn't clear that this function is potentially unsafe
    Bank.withdraw(amount);
}

// good
UntrustedBank.withdraw(100); // untrusted external call
TrustedBank.withdraw(100); // external but trusted bank contract maintained by XYZ Corp

function makeUntrustedWithdrawal(uint amount) {
    UntrustedBank.withdraw(amount);
}
</code></pre>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h4 id="avoid-state-changes-after-external-calls">Avoid state changes after external calls</h4>



<p>Whether using&nbsp;<em>raw calls</em>&nbsp;(of the form&nbsp;<code>someAddress.call()</code>) or&nbsp;<em>contract calls</em>&nbsp;(of the form&nbsp;<code>ExternalContract.someMethod()</code>), assume that malicious code might execute. Even if&nbsp;<code>ExternalContract</code>&nbsp;is not malicious, malicious code can be executed by any contracts&nbsp;<em>it</em>&nbsp;calls.</p>



<p>One particular danger is malicious code may hijack the control flow, leading to vulnerabilities due to reentrancy. (See&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/attacks/reentrancy/">Reentrancy</a>&nbsp;for a fuller discussion of this problem).</p>



<p>If you are making a call to an untrusted external contract,&nbsp;<em>avoid state changes after the call</em>. This pattern is also sometimes known as the&nbsp;<a href="http://solidity.readthedocs.io/en/develop/security-considerations.html?highlight=check%20effects#use-the-checks-effects-interactions-pattern">checks-effects-interactions pattern</a>.</p>



<p>See&nbsp;<a href="https://swcregistry.io/docs/SWC-107">SWC-107</a></p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h4 id="dont-use-transfer-or-send">Don’t use&nbsp;<code>transfer()</code>&nbsp;or&nbsp;<code>send()</code>.</h4>



<p><code>.transfer()</code>&nbsp;and&nbsp;<code>.send()</code>&nbsp;forward exactly 2,300 gas to the recipient. The goal of this hardcoded gas stipend was to prevent&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/attacks/reentrancy/">reentrancy vulnerabilities</a>, but this only makes sense under the assumption that gas costs are constant. Recently&nbsp;<a href="https://eips.ethereum.org/EIPS/eip-1884">EIP 1884</a>&nbsp;was included in the Istanbul hard fork. One of the changes included in EIP 1884 is an increase to the gas cost of the&nbsp;<code>SLOAD</code>&nbsp;operation, causing a contract’s fallback function to cost more than 2300 gas.</p>



<p>It’s recommended to stop using&nbsp;<code>.transfer()</code>&nbsp;and&nbsp;<code>.send()</code>&nbsp;and instead use&nbsp;<code>.call()</code>.</p>



<pre id="__code_2" class="wp-block-code"><code>// bad
contract Vulnerable {
    function withdraw(uint256 amount) external {
        // This forwards 2300 gas, which may not be enough if the recipient
        // is a contract and gas costs change.
        msg.sender.transfer(amount);
    }
}

// good
contract Fixed {
    function withdraw(uint256 amount) external {
        // This forwards all available gas. Be sure to check the return value!
        (bool success, ) = msg.sender.call.value(amount)("");
        require(success, "Transfer failed.");
    }
}
</code></pre>



<p>Note that&nbsp;<code>.call()</code>&nbsp;does nothing to mitigate reentrancy attacks, so other precautions must be taken. To prevent reentrancy attacks, it is recommended that you use the&nbsp;<a href="https://solidity.readthedocs.io/en/develop/security-considerations.html?highlight=check%20effects#use-the-checks-effects-interactions-pattern">checks-effects-interactions pattern</a>.</p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h4 id="handle-errors-in-external-calls">Handle errors in external calls</h4>



<p>Solidity offers low-level call methods that work on raw addresses:&nbsp;<code>address.call()</code>,&nbsp;<code>address.callcode()</code>,&nbsp;<code>address.delegatecall()</code>, and&nbsp;<code>address.send()</code>. These low-level methods never throw an exception, but will return&nbsp;<code>false</code>&nbsp;if the call encounters an exception. On the other hand,&nbsp;<em>contract calls</em>&nbsp;(e.g.,&nbsp;<code>ExternalContract.doSomething()</code>) will automatically propagate a throw (for example,&nbsp;<code>ExternalContract.doSomething()</code>&nbsp;will also&nbsp;<code>throw</code>&nbsp;if&nbsp;<code>doSomething()</code>&nbsp;throws).</p>



<p>If you choose to use the low-level call methods, make sure to handle the possibility that the call will fail, by checking the return value.</p>



<pre id="__code_3" class="wp-block-code"><code>// bad
someAddress.send(55);
someAddress.call.value(55)(""); // this is doubly dangerous, as it will forward all remaining gas and doesn't check for result
someAddress.call.value(100)(bytes4(sha3("deposit()"))); // if deposit throws an exception, the raw call() will only return false and transaction will NOT be reverted

// good
(bool success, ) = someAddress.call.value(55)("");
if(!success) {
    // handle failure code
}

ExternalContract(someAddress).deposit.value(100)();
</code></pre>



<p>See&nbsp;<a href="https://swcregistry.io/docs/SWC-104">SWC-104</a></p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h4 id="favor-pull-over-push-for-external-calls">Favor&nbsp;<em>pull</em>&nbsp;over&nbsp;<em>push</em>&nbsp;for external calls</h4>



<p>External calls can fail accidentally or deliberately. To minimize the damage caused by such failures, it is often better to isolate each external call into its own transaction that can be initiated by the recipient of the call. This is especially relevant for payments, where it is better to let users withdraw funds rather than push funds to them automatically. (This also reduces the chance of&nbsp;<a href="https://consensys.github.io/smart-contract-best-practices/attacks/denial-of-service/">problems with the gas limit</a>.) Avoid combining multiple ether transfers in a single transaction.</p>



<pre id="__code_4" class="wp-block-code"><code>// bad
contract auction {
    address highestBidder;
    uint highestBid;

    function bid() payable {
        require(msg.value &gt;= highestBid);

        if (highestBidder != address(0)) {
            (bool success, ) = highestBidder.call.value(highestBid)("");
            require(success); // if this call consistently fails, no one else can bid
        }

       highestBidder = msg.sender;
       highestBid = msg.value;
    }
}

// good
contract auction {
    address highestBidder;
    uint highestBid;
    mapping(address =&gt; uint) refunds;

    function bid() payable external {
        require(msg.value &gt;= highestBid);

        if (highestBidder != address(0)) {
            refunds[highestBidder] += highestBid; // record the refund that this user can claim
        }

        highestBidder = msg.sender;
        highestBid = msg.value;
    }

    function withdrawRefund() external {
        uint refund = refunds[msg.sender];
        refunds[msg.sender] = 0;
        (bool success, ) = msg.sender.call.value(refund)("");
        require(success);
    }
}
</code></pre>



<p>See&nbsp;<a href="https://swcregistry.io/docs/SWC-128">SWC-128</a></p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<h4 id="dont-delegatecall-to-untrusted-code">Don’t delegatecall to untrusted code</h4>



<p>The&nbsp;<code>delegatecall</code>&nbsp;function is used to call functions from other contracts as if they belong to the caller contract. Thus the callee may change the state of the calling address. This may be insecure. An example below shows how using&nbsp;<code>delegatecall</code>&nbsp;can lead to the destruction of the contract and loss of its balance.</p>



<pre id="__code_5" class="wp-block-code"><code>contract Destructor
{
    function doWork() external
    {
        selfdestruct(0);
    }
}

contract Worker
{
    function doWork(address _internalWorker) public
    {
        // unsafe
        _internalWorker.delegatecall(bytes4(keccak256("doWork()")));
    }
}
</code></pre>



<p>If&nbsp;<code>Worker.doWork()</code>&nbsp;is called with the address of the deployed&nbsp;<code>Destructor</code>&nbsp;contract as an argument, the&nbsp;<code>Worker</code>&nbsp;contract will self-destruct. Delegate execution only to trusted contracts, and&nbsp;never to a user supplied address.</p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<p class="has-background" style="background-color:#f78da812">Front-running is a pervasive issue in Ethereum DApps. DApp developers don’t necessarily have the mindset to design DApps with front-running in mind. This is an attempt to bring forward the subject and increase awareness of these type of attacks. While some DApp-level application logic could be built to mitigate these attacks, its ubiquity across different DApp categories suggests mitigations at the blockchain-level would perhaps be more effective. We highlight this as an important research area. We consider front-running to be a course of action where an entity benefits from prior access to privileged market information about upcoming transactions and trades. Front-running has been an issue in financial instrument markets since the 1970s. With the advent of the blockchain technology, front-running has resurfaced in new forms we explore here, instigated by blockchain’s decentralized and transparent nature. In this paper, we draw from a scattered body of knowledge and instances of front-running across the top 25 most active decentral applications (DApps) deployed on Ethereum blockchain. Additionally, we carry out a detailed analysis of Status.im initial coin offering (ICO) and show evidence of abnormal miner’s behavior indicative of front-running token purchases. Finally, we map the proposed solutions to front-running into useful categories</p>



<hr class="wp-block-separator has-alpha-channel-opacity">



<p><strong><a href="https://github.com/demining/Improving-Overall-Security" target="_blank" rel="noreferrer noopener">GitHub</a></strong></p>



<p><strong><a href="https://t.me/cryptodeeptech" target="_blank" rel="noreferrer noopener">Telegram:&nbsp;https://t.me/cryptodeeptech</a></strong></p>



<p><a href="https://youtu.be/HVh_cbsgSMg" target="_blank" rel="noreferrer noopener"><strong>Video: https://youtu.be/HVh_cbsgSMg</strong></a></p>



<p><strong><a href="https://cryptodeeptech.ru/improving-overall-security" target="_blank" rel="noreferrer noopener">Source: https://cryptodeeptech.ru/improving-overall-security</a></strong></p>



<hr class="wp-block-separator has-alpha-channel-opacity">


<div class="wp-block-image">
<figure class="aligncenter size-large"><img decoding="async" loading="lazy" width="1024" height="576" src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/035-1-1024x576.png" alt="Improving the overall security of the ecosystem from attacks on smart contracts" class="wp-image-1999" srcset="https://cryptodeeptech.ru/wp-content/uploads/2023/03/035-1-1024x576.png 1024w, https://cryptodeeptech.ru/wp-content/uploads/2023/03/035-1-300x169.png 300w, https://cryptodeeptech.ru/wp-content/uploads/2023/03/035-1-768x432.png 768w, https://cryptodeeptech.ru/wp-content/uploads/2023/03/035-1.png 1280w" sizes="(max-width: 1024px) 100vw, 1024px"></figure></div>


<p></p>



<p></p>
	</div><!-- .entry-content -->

	<footer class="entry-footer">
		<div class="cat-links"><i class="fa fa-folder-open" aria-hidden="true"></i> <a href="https://cryptodeeptech.ru/category/cryptanalysis/" rel="category tag">Cryptanalysis</a></div><div class="edit-link"><a class="post-edit-link" href="https://cryptodeeptech.ru/wp-admin/post.php?post=1993&amp;action=edit">Edit <span class="screen-reader-text">Improving the overall security of the ecosystem from attacks on smart contracts</span></a></div>	</footer><!-- .entry-footer -->
</article><!-- #post-1993 -->

	<nav class="navigation post-navigation" aria-label="Записи">
		<h2 class="screen-reader-text">Навигация по записям</h2>
		<div class="nav-links"><div class="nav-previous"><a href="https://cryptodeeptech.ru/twist-attack-2/" rel="prev">Twist Attack example №2 continue a series of ECC operations to get the value of Private Key to the Bitcoin Wallet</a></div></div>
	</nav>		<div id="itng_related_posts_wrapper">
			<h3 id="itng_related_posts_title">Related Posts</h3>
			<div class="itng-related-posts row">
				<article id="post-1708" class="itng-blog col-md-6 col-lg-4 post-1708 post type-post status-private format-standard hentry category-cryptanalysis">
		<div class="itng-card-wrapper">
			<div class="itng-thumb">
							</div>
			
			<div class="itng-card-content">
				<div class="entry-meta">
					<a href="https://cryptodeeptech.ru/vulnerabilities-2-function-default-visibility/"></a>
					<span class="byline"> <span class="author vcard"><a class="url fn n" href="https://cryptodeeptech.ru/author/cryptodeeptech/">Crypto Deep Tech</a></span></span>				</div><!-- .entry-meta -->
				
				<header class="entry-header">
					<h2 class="entry-title"><a href="https://cryptodeeptech.ru/vulnerabilities-2-function-default-visibility/">Личное: Vulnerabilities 2 Function Default Visibility</a></h2>				</header><!-- .entry-header -->
				
				<div class="itng_excerpt">
					Function Default Visibility Function visibility can be specified as either: public, private, internal, or external. It's important to consider which visibility is best for your smart contract function. Many smart contract attacks are caused by a developer forgetting or forgoing to use a visibility modifier. The function is then set as public by default, which can lead…				</div>
				
				<div class="blog-footer">
					<div class="itng_cats">
						<a href="https://cryptodeeptech.ru/category/cryptanalysis/" rel="category tag">Cryptanalysis</a>					</div>
					<div class="blog-comments">
						0					</div>
				</div>
			</div>
		</div>
</article><!-- #post-1708 --><article id="post-1219" class="itng-blog col-md-6 col-lg-4 post-1219 post type-post status-publish format-standard hentry category-cryptanalysis">
		<div class="itng-card-wrapper">
			<div class="itng-thumb">
							</div>
			
			<div class="itng-card-content">
				<div class="entry-meta">
					<a href="https://cryptodeeptech.ru/defi-attacks/"></a>
					<span class="byline"> <span class="author vcard"><a class="url fn n" href="https://cryptodeeptech.ru/author/cryptodeeptech/">Crypto Deep Tech</a></span></span>				</div><!-- .entry-meta -->
				
				<header class="entry-header">
					<h2 class="entry-title"><a href="https://cryptodeeptech.ru/defi-attacks/">DeFi Attacks &amp; Exploits all the biggest cryptocurrency thefts from 2021 to 2022</a></h2>				</header><!-- .entry-header -->
				
				<div class="itng_excerpt">
					In this article, we will tell you about the most daring and biggest thefts of cryptocurrencies associated with&nbsp;платформой DeFi.&nbsp;Hackers had a big year in 2021 when they stole $3.2 billion worth of cryptocurrencies.&nbsp;But in 2022, the amount of theft of cryptocurrencies reached a historical maximum.&nbsp;In the first three months of this year, hackers have stolen $1.3 billion&nbsp;from…				</div>
				
				<div class="blog-footer">
					<div class="itng_cats">
						<a href="https://cryptodeeptech.ru/category/cryptanalysis/" rel="category tag">Cryptanalysis</a>					</div>
					<div class="blog-comments">
						0					</div>
				</div>
			</div>
		</div>
</article><!-- #post-1219 --><article id="post-1692" class="itng-blog col-md-6 col-lg-4 post-1692 post type-post status-private format-standard hentry category-cryptanalysis">
		<div class="itng-card-wrapper">
			<div class="itng-thumb">
							</div>
			
			<div class="itng-card-content">
				<div class="entry-meta">
					<a href="https://cryptodeeptech.ru/dao-exploit/"></a>
					<span class="byline"> <span class="author vcard"><a class="url fn n" href="https://cryptodeeptech.ru/author/cryptodeeptech/">Crypto Deep Tech</a></span></span>				</div><!-- .entry-meta -->
				
				<header class="entry-header">
					<h2 class="entry-title"><a href="https://cryptodeeptech.ru/dao-exploit/">Личное: Cryptanalysis of the DAO exploit &amp;  Multi-Stage Attack</a></h2>				</header><!-- .entry-header -->
				
				<div class="itng_excerpt">
					This post will be the first in what is potentially a series, deconstructing and explaining what went wrong at the technical level while providing a timeline tracing the actions of the attacker back through the blockchain. This first post will focus on&nbsp;how&nbsp;exactly the attacker stole all the money in the DAO. A Multi-Stage Attack This exploit in…				</div>
				
				<div class="blog-footer">
					<div class="itng_cats">
						<a href="https://cryptodeeptech.ru/category/cryptanalysis/" rel="category tag">Cryptanalysis</a>					</div>
					<div class="blog-comments">
						0					</div>
				</div>
			</div>
		</div>
</article><!-- #post-1692 -->			</div>
		</div>
			<div id="author_box" class="row no-gutters">
			<div class="author_avatar col-2">
							</div>
			<div class="author_info col-10">
				<h4 class="author_name title-font">
					Crypto Deep Tech				</h4>
				<div class="author_bio">
									</div>
			</div>
		</div>
	
	</main><!-- #main -->

</div><!-- #content-wrapper -->


 <div id="footer-sidebar" class="widget-area">
    <div class="container">
        <div class="row">
                    </div>
    </div>
</div>
	<footer id="colophon" class="site-footer">
		<div class="container">
			<div class="site-info">
				Donation Address: <a href="https://www.blockchain.com/btc/address/1Lw2gTnMpxRUNBU85Hg4ruTwnpUPKdf3nV" target="_blank">♥  BTC: 1Lw2gTnMpxRUNBU85Hg4ruTwnpUPKdf3nV</a>				<span class="sep"> | </span>
					Copyright © 2023 «CRYPTO DEEP TECH». 			</div><!-- .site-info -->
		</div>
	</footer><!-- #colophon -->
</div><!-- #page -->

<nav id="menu" class="panel" role="navigation" style="position: fixed; top: 0px; bottom: 0px; height: 100%; left: -15.625em; width: 15.625em;">
	<div class="menu-overlay"></div>
	<div id="panel-top-bar">
		<button class="go-to-bottom"></button>
		<button id="close-menu" class="menu-link"><i class="fa fa-chevron-left" aria-hidden="true"></i></button>
	</div>

	<ul id="menu-main" class="menu"><li id="menu-item-229" class="menu-item menu-item-type-custom menu-item-object-custom menu-item-home"><a href="https://cryptodeeptech.ru/">HOME</a></li>
<li id="menu-item-225" class="menu-item menu-item-type-post_type menu-item-object-page"><a href="https://cryptodeeptech.ru/publication/">PUBLICATIONS</a></li>
<li id="menu-item-226" class="menu-item menu-item-type-post_type menu-item-object-page"><a href="https://cryptodeeptech.ru/study/">STUDY</a></li>
<li id="menu-item-227" class="menu-item menu-item-type-post_type menu-item-object-page"><a href="https://cryptodeeptech.ru/resources/">RESOURCES</a></li>
<li id="menu-item-228" class="menu-item menu-item-type-post_type menu-item-object-page"><a href="https://cryptodeeptech.ru/contacts/">CONTACTS</a></li>
<li id="menu-item-240" class="menu-item menu-item-type-post_type menu-item-object-post"><a href="https://cryptodeeptech.ru/lattice-attack/">BLOG</a></li>
<li id="menu-item-541" class="menu-item menu-item-type-post_type menu-item-object-page"><a href="https://cryptodeeptech.ru/eng/">ENG</a></li>
<li id="menu-item-542" class="menu-item menu-item-type-post_type menu-item-object-page"><a href="https://cryptodeeptech.ru/rus/">RUS</a></li>
<li id="menu-item-1974" class="menu-item menu-item-type-post_type menu-item-object-page menu-item-has-children"><a href="https://cryptodeeptech.ru/other-languages/">Other Languages</a><span class="dropdown-arrow" tabindex="0"><i class="fa fa-angle-down"></i></span>
<ul class="sub-menu" style="display: none;">
	<li id="menu-item-1975" class="menu-item menu-item-type-post_type menu-item-object-page"><a href="https://cryptodeeptech.ru/cn/">CN</a></li>
	<li id="menu-item-1992" class="menu-item menu-item-type-post_type menu-item-object-page"><a href="https://cryptodeeptech.ru/kr/">KR</a></li>
</ul>
</li>
</ul>
	<button class="go-to-top"></button>
</nav>

<div id="sticky-navigation">
	<div class="nav-wrapper">
		 <div class="container">

			 <div class="row justify-content-end align-items-center justify-content-between no-gutters">


				<div class="main-navigation col-lg-9" role="navigation">
					<ul id="menu-desktop" class="menu"><li class="menu-item menu-item-type-custom menu-item-object-custom menu-item-home menu-item-229"><a href="https://cryptodeeptech.ru/">HOME</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-225"><a href="https://cryptodeeptech.ru/publication/">PUBLICATIONS</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-226"><a href="https://cryptodeeptech.ru/study/">STUDY</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-227"><a href="https://cryptodeeptech.ru/resources/">RESOURCES</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-228"><a href="https://cryptodeeptech.ru/contacts/">CONTACTS</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-post menu-item-240"><a href="https://cryptodeeptech.ru/lattice-attack/">BLOG</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-541"><a href="https://cryptodeeptech.ru/eng/">ENG</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-542"><a href="https://cryptodeeptech.ru/rus/">RUS</a></li>
<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-has-children menu-item-1974"><a href="https://cryptodeeptech.ru/other-languages/">Other Languages</a>
<ul class="sub-menu">
	<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-1975"><a href="https://cryptodeeptech.ru/cn/">CN</a></li>
	<li class="menu-item menu-item-type-post_type menu-item-object-page menu-item-1992"><a href="https://cryptodeeptech.ru/kr/">KR</a></li>
</ul>
</li>
</ul>				</div>

				<button href="#menu" class="menu-link mobile-nav-btn"><i class="fa fa-bars" aria-hidden="true"></i></button>

				<button type="button" id="go-to-field" tabindex="-1"></button>

				<button class="search-btn-sticky ml-auto col-auto"><i class="fa fa-search"></i></button>
				
<div class="itng-search-sticky">
	<form role="search" method="get" class="search-form" action="https://cryptodeeptech.ru/">
				<label>
					<span class="screen-reader-text">Найти:</span>
					<input type="search" class="search-field" placeholder="Поиск…" value="" name="s">
				</label>
				<input type="submit" class="search-submit" value="Поиск">
			</form>	<button type="button" id="go-to-btn" tabindex="-1"></button>
</div>

			</div>
		</div>
	</div>
</div>

<div id="itng-back-to-top" class="show"><i class="fa fa-chevron-up" aria-hidden="true"></i></div>

		<script type="text/javascript">
							jQuery("#post-1993 .entry-meta .date").css("display","none");
					jQuery("#post-1993 .entry-date").css("display","none");
					jQuery("#post-1993 .posted-on").css("display","none");
							jQuery("#post-1708 .entry-meta .date").css("display","none");
					jQuery("#post-1708 .entry-date").css("display","none");
					jQuery("#post-1708 .posted-on").css("display","none");
							jQuery("#post-1219 .entry-meta .date").css("display","none");
					jQuery("#post-1219 .entry-date").css("display","none");
					jQuery("#post-1219 .posted-on").css("display","none");
							jQuery("#post-1692 .entry-meta .date").css("display","none");
					jQuery("#post-1692 .entry-date").css("display","none");
					jQuery("#post-1692 .posted-on").css("display","none");
				</script>
				<script type="text/javascript">
				var wpfc_ajaxurl = "https://cryptodeeptech.ru/wp-admin/admin-ajax.php";
				var wpfc_nonce = "33eb9fc5a4";
			</script>
			<script src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/hoverintent-js.min.js" id="hoverintent-js-js"></script>
<script src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/admin-bar.min.js" id="admin-bar-js"></script>
<script src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/bigSlide.js" id="big-slide-js"></script>
<script src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/owl.carousel.js" id="owl-js-js"></script>
<script src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/jquery.magnific-popup.min.js" id="mag-lightbox-js-js"></script>
<script id="itng-custom-js-js-extra">
var itng = {"toTopEnable":"1","stickyNav":""};
</script>
<script src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/custom.js" id="itng-custom-js-js"></script>
<script src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/navigation.js" id="itng-navigation-js"></script>
<script src="./Improving the overall security of the ecosystem from attacks on smart contracts - CRYPTO DEEP TECH_files/toolbar.js" id="wpfc-toolbar-js"></script>

<!-- Yandex.Metrika counter -->
<script type="text/javascript">
   (function(m,e,t,r,i,k,a){m[i]=m[i]||function(){(m[i].a=m[i].a||[]).push(arguments)};
   var z = null;m[i].l=1*new Date();
   for (var j = 0; j < document.scripts.length; j++) {if (document.scripts[j].src === r) { return; }}
   k=e.createElement(t),a=e.getElementsByTagName(t)[0],k.async=1,k.src=r,a.parentNode.insertBefore(k,a)})
   (window, document, "script", "https://mc.yandex.ru/metrika/tag.js", "ym");

   ym(89995532, "init", {
        clickmap:true,
        trackLinks:true,
        accurateTrackBounce:true,
        webvisor:true
   });
</script>
<noscript><div><img src="https://mc.yandex.ru/watch/89995532" style="position:absolute; left:-9999px;" alt="" /></div></noscript>
<!-- /Yandex.Metrika counter -->




<!--
Performance optimized by W3 Total Cache. Learn more: https://www.boldgrid.com/w3-total-cache/

Кэширование страницы с использованием disk: enhanced (User is logged in) 

Served from: cryptodeeptech.ru @ 2023-03-16 23:25:51 by W3 Total Cache
--><div id="revert-loader-toolbar"></div></body></html>
