<script>
    import { ethers } from 'ethers';
  
    let provider;
    let signer;
    let account;
    let contract;
  
    // Import the contract ABI and contract address
    import contractABI from './contractABI.json';
    const contractAddress = '0x226074b8FdBF962370E08948BBd9AFc828c08363';
  
    export async function connectWallet() {
      // Check if the user has an Ethereum-compatible browser
      if (window.ethereum) {
        try {
          // Request the user's permission to connect to their wallet
          await window.ethereum.request({ method: 'eth_requestAccounts' });
          provider = new ethers.providers.Web3Provider(window.ethereum);
          signer = provider.getSigner();
          account = await signer.getAddress();
          console.log('Connected to wallet:', account);
  
          // Create an instance of the contract
          contract = new ethers.Contract(contractAddress, contractABI, signer);
        } catch (error) {
          console.log(error);
        }
      } else {
        console.log('No Ethereum-compatible browser detected.');
      }
    }
  
    export async function requestTokens() {
      // Call the requestTokens function of the contract
      await contract.requestTokens();
    }
  </script>
  
  <main>
    <h1>AuditUtils AUtoken faucet</h1>
  
    <p style="color: blue; font-size: 30px;">
      Pick up your AUtoken by connecting your wallet and making a request. You
      will only need to pay gas fees (GETH) to receive 1000 AUtoken.
    </p>
  
    <button on:click={connectWallet}>Connect Wallet</button>
    <button on:click={requestTokens}>Request Token</button>
  
    <p style="color: green; font-size: 30px;">
      You might use them in those demo apps on goerli test net:
    </p>
  
    <p>Read contract functions</p>
    <a href="https://goerli.etherscan.io/readContract?m=light&a=0x226074b8FdBF962370E08948BBd9AFc828c08363&n=goerli&v=0x226074b8FdBF962370E08948BBd9AFc828c08363#readCollapse1">"Etherscan Read Contract"</a>
  
    <p>Write contract functions</p>
    <a href="https://goerli.etherscan.io/writecontract/index?m=light&v=21.10.1.1&a=0x226074b8FdBF962370E08948BBd9AFc828c08363&n=goerli&p=#collapse1">"Etherscan Write Contract"</a>
  
    <p>View contract on Etherscan</p>
    <a href="https://goerli.etherscan.io/address/0x2788B56Ef7aE6F1cc95f74a91E5f91bE9bF4367e">"Etherscan Contract"</a>
  </main>