<template>
  <div class="hello">
    <button @click="sendPudding()">プリンコインをおくる</button>
    <!-- <input v-model="sendee" placeholder="input sendee">
    <input v-model="price" placeholder="input price"> -->
  </div>
</template>

<script>
import send from './send'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  methods: {
    sendPudding(){
    // Dotenv javascript libraries needed
    // require('dotenv').config();
    // Ethereum javascript libraries needed
    var Web3 = require('web3');
    var Tx = require('ethereumjs-tx');
    // Rather than using a local copy of geth, interact with the ethereum blockchain via infura.io
    // The key for infura.io is in .env
    const web3 = new Web3(Web3.givenProvider || "https://ropsten.infura.io/jskHoqnOn6oW7kZvhVMu")
    // Fixed-point notation for number of MFIL which is divisible to 3 decimal places
    function financialMfil(numMfil) {
        return Number.parseFloat(numMfil / 1e3).toFixed(3);
    }
    // Create an async function so I can use the "await" keyword to wait for things to finish
    const main = async () => {
        // This code was written and tested using web3 version 1.0.0-beta.29
        console.log(`web3 version: ${web3.version}`)


        // Who holds the token now?
        var myAddress = "0x1d134468D686c864E494fA55171ab402D7b816C3";
        
        // Who are we trying to send this token to?
        var destAddress = "0x5582df44012a510d16399c2e5c03ac2f249fbce4";
        // var destAddress = sendee;


        // MineFIL Token (MFIL) is divisible to 3 decimal places, 1 = 0.001 of MFIL
        var transferAmount = 100e18;
        // Determine the nonce
        var count = await web3.eth.getTransactionCount(myAddress);
        console.log(`num transactions so far: ${count}`);
        // MineFILToekn contract ABI Array
        var abiArray = [{
            "constant": true,
            "inputs": [],
            "name": "name",
            "outputs": [{
                "name": "",
                "type": "string"
            }],
            "payable": false,
            "stateMutability": "view",
            "type": "function"
        }, {
            "constant": false,
            "inputs": [{
                "name": "_spender",
                "type": "address"
            }, {
                "name": "_value",
                "type": "uint256"
            }],
            "name": "approve",
            "outputs": [{
                "name": "",
                "type": "bool"
            }],
            "payable": false,
            "stateMutability": "nonpayable",
            "type": "function"
        }, {
            "constant": true,
            "inputs": [],
            "name": "totalSupply",
            "outputs": [{
                "name": "",
                "type": "uint256"
            }],
            "payable": false,
            "stateMutability": "view",
            "type": "function"
        }, {
            "constant": false,
            "inputs": [{
                "name": "_from",
                "type": "address"
            }, {
                "name": "_to",
                "type": "address"
            }, {
                "name": "_value",
                "type": "uint256"
            }],
            "name": "transferFrom",
            "outputs": [{
                "name": "",
                "type": "bool"
            }],
            "payable": false,
            "stateMutability": "nonpayable",
            "type": "function"
        }, {
            "constant": true,
            "inputs": [],
            "name": "INITIAL_SUPPLY",
            "outputs": [{
                "name": "",
                "type": "uint256"
            }],
            "payable": false,
            "stateMutability": "view",
            "type": "function"
        }, {
            "constant": true,
            "inputs": [],
            "name": "decimals",
            "outputs": [{
                "name": "",
                "type": "uint8"
            }],
            "payable": false,
            "stateMutability": "view",
            "type": "function"
        }, {
            "constant": false,
            "inputs": [{
                "name": "_spender",
                "type": "address"
            }, {
                "name": "_subtractedValue",
                "type": "uint256"
            }],
            "name": "decreaseApproval",
            "outputs": [{
                "name": "",
                "type": "bool"
            }],
            "payable": false,
            "stateMutability": "nonpayable",
            "type": "function"
        }, {
            "constant": true,
            "inputs": [{
                "name": "_owner",
                "type": "address"
            }],
            "name": "balanceOf",
            "outputs": [{
                "name": "balance",
                "type": "uint256"
            }],
            "payable": false,
            "stateMutability": "view",
            "type": "function"
        }, {
            "constant": true,
            "inputs": [],
            "name": "symbol",
            "outputs": [{
                "name": "",
                "type": "string"
            }],
            "payable": false,
            "stateMutability": "view",
            "type": "function"
        }, {
            "constant": false,
            "inputs": [{
                "name": "_to",
                "type": "address"
            }, {
                "name": "_value",
                "type": "uint256"
            }],
            "name": "transfer",
            "outputs": [{
                "name": "",
                "type": "bool"
            }],
            "payable": false,
            "stateMutability": "nonpayable",
            "type": "function"
        }, {
            "constant": false,
            "inputs": [{
                "name": "_spender",
                "type": "address"
            }, {
                "name": "_addedValue",
                "type": "uint256"
            }],
            "name": "increaseApproval",
            "outputs": [{
                "name": "",
                "type": "bool"
            }],
            "payable": false,
            "stateMutability": "nonpayable",
            "type": "function"
        }, {
            'constant': true,
            "inputs": [{
                "name": "_owner",
                "type": "address"
            }, {
                "name": "_spender",
                "type": "address"
            }],
            "name": "allowance",
            "outputs": [{
                "name": "",
                "type": "uint256"
            }],
            "payable": false,
            "stateMutability": "view",
            "type": "function"
        }, {
            "inputs": [],
            "payable": false,
            "stateMutability": "nonpayable",
            "type": "constructor"
        }, {
            "anonymous": false,
            "inputs": [{
                "indexed": true,
                "name": "owner",
                "type": "address"
            }, {
                "indexed": true,
                "name": "spender",
                "type": "address"
            }, {
                "indexed": false,
                "name": "value",
                "type": "uint256"
            }],
            "name": "Approval",
            "type": "event"
        }, {
            "anonymous": false,
            "inputs": [{
                "indexed": true,
                "name": "from",
                "type": "address"
            }, {
                "indexed": true,
                "name": "to",
                "type": "address"
            }, {
                "indexed": false,
                "name": "value",
                "type": "uint256"
            }],
            "name": "Transfer",
            "type": "event"
        }]
        // The address of the contract which created MFIL
        var contractAddress = "0x63a3821c289316026d06d70fb1fe47587a836494";
        var contract = new web3.eth.Contract(abiArray, contractAddress, {
            from: myAddress
        });
        // How many tokens do I have before sending?
        var balance = await contract.methods.balanceOf(myAddress).call();
        console.log(`Balance before send: ${financialMfil(balance)} PDN\n------------------------`);
        // I chose gas price and gas limit based on what ethereum wallet was recommending for a similar transaction. You may need to change the gas price!
        // Use Gwei for the unit of gas price
        var gasPriceGwei =  3000000;
        var gasLimit = 2000000;
        // Chain ID of Ropsten Test Net is 3, replace it to 1 for Main Net
        var chainId = 3;
        var rawTransaction = {
            "from": myAddress,
            "nonce": "0x" + count.toString(16),
            "gasPrice": web3.utils.toHex(gasPriceGwei),
            "gasLimit": web3.utils.toHex(gasLimit),
            "to": contractAddress,
            "value":"0x0",
            "data": contract.methods.transfer(destAddress, transferAmount).encodeABI(),
            "chainId": chainId
        };
        console.log(`Raw of Transaction: \n${JSON.stringify(rawTransaction, null, '\t')}\n------------------------`);
        // The private key for myAddress in .env
        var privKey = new Buffer("e6898fb50261a64aa9207a7da2fe0bfee9aa63e24a9bb70a1342ab14c68e0f2f", 'hex');
        var tx = new Tx(rawTransaction);
        tx.sign(privKey);
        var serializedTx = tx.serialize();
        // Comment out these four lines if you don't really want to send the TX right now
        console.log(`Attempting to send signed tx:  ${serializedTx.toString('hex')}\n------------------------`);
        var receipt = await web3.eth.sendSignedTransaction('0x' + serializedTx.toString('hex'));
        // The receipt info of transaction, Uncomment for debug
        console.log(`Receipt info: \n${JSON.stringify(receipt, null, '\t')}\n------------------------`);
        // The balance may not be updated yet, but let's check
        balance = await contract.methods.balanceOf(myAddress).call();
        console.log(`Balance after send: ${financialMfil(balance)} MFIL`);
    }
    main();
        }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
