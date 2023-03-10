import { ethers } from 'ethers';

const alchemyEndpoint = 'https://eth-goerli.alchemyapi.io/v2/Q3spMRBoqI1YrRBG7_YH6a6DWToyP6nP';
const provider = new ethers.providers.JsonRpcProvider(alchemyEndpoint);

const contractAddress = '0x226074b8FdBF962370E08948BBd9AFc828c08363';
const auditUtilsTokenFaucetContract = new ethers.Contract(contractAddress, auditUtilsTokenFaucetABI, provider);

const accounts = await provider.listAccounts();
await auditUtilsTokenFaucetContract.connect(provider.getSigner(accounts[0])).requestTokens();


<script>
    import { ethers } from 'ethers';
    import auditUtilsTokenFaucetABI from './auditUtilsTokenFaucet.json';
  
    let provider;
    let signer;
    let contract;
    let account;
    let balance;
  
    async function connectWallet() {
      try {
        // Prompt the user to connect to a wallet
        provider = new ethers.providers.Web3Provider(window.ethereum);
        await provider.send('eth_requestAccounts', []);
        signer = provider.getSigner();
        contract = new ethers.Contract('0x226074b8FdBF962370E08948BBd9AFc828c08363', auditUtilsTokenFaucetABI, signer);
        account = await signer.getAddress();
        balance = await contract.balanceOf(account);
      } catch (err) {
        console.error(err);
      }
    }
  
    async function setAmount(newAmount) {
      try {
        await contract.setAmount(newAmount);
        console.log('Amount set to', newAmount);
      } catch (err) {
        console.error(err);
      }
    }
  
    async function setInterval(newInterval) {
      try {
        await contract.setInterval(newInterval);
        console.log('Interval set to', newInterval);
      } catch (err) {
        console.error(err);
      }
    }
  
    async function requestTokens() {
      try {
        await contract.requestTokens();
        console.log('Tokens requested successfully');
      } catch (err) {
        console.error(err);
      }
    }
  </script>
  
  <main>
    {#if !account}
      <button on:click={connectWallet}>Connect Wallet</button>
    {:else}
      <p>Account: {account}</p>
      <p>Balance: {balance.toString()} tokens</p>
      <button on:click={() => setAmount(5 * 10**18)}>Set Amount to 5 tokens</button>
      <button on:click={() => setAmount(10 * 10**18)}>Set Amount to 10 tokens</button>
      <button on:click={() => setInterval(5 * 60)}>Set Interval to 5 minutes</button>
      <button on:click={() => setInterval(10 * 60)}>Set Interval to 10 minutes</button>
      <button on:click={requestTokens}>Request Tokens</button>
    {/if}
  </main>