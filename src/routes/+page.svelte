<script>
  import Web3 from "web3";
  import AuditUtilsTokenFaucetABI from "./AuditUtilsTokenFaucetABI.json";

  const contractAddress = "0x226074b8FdBF962370E08948BBd9AFc828c08363";

  let web3;
  let contract;

  export async function connectWallet() {
    if (window.ethereum) {
      try {
        await window.ethereum.request({ method: "eth_requestAccounts" });
        web3 = new Web3(window.ethereum);

        const accounts = await web3.eth.getAccounts();
        console.log("Connected to wallet:", accounts[0]);

        contract = new web3.eth.Contract(
          AuditUtilsTokenFaucetABI,
          contractAddress
        );
      } catch (error) {
        console.log(error);
      }
    } else {
      console.log("No Ethereum-compatible browser detected.");
    }
  }

  export async function requestTokens() {
    if (contract) {
      await contract.methods
        .requestTokens()
        .send({ from: (await web3.eth.getAccounts())[0] });
    } else {
      console.log("Contract not initialized.");
    }
  }
</script>

<main>
  <head>
    <link rel="stylesheet" href="/style.css" />
    <!-- Add Google Fonts for the webpage -->
    <link
      href="https://fonts.googleapis.com/css?family=Press+Start+2P&display=swap"
      rel="stylesheet"
    />
  </head>
  <div class="container">
    <div>
      <a
        style="margin: 30px;
      padding: 12px;
      border-radius: 15px;
      border: 6px solid rgba(10, 202, 166, 0.9);
      
      width: auto;"
        href="https://user137-portfolio.auditutils.com/"
      >
        <img
          style="max-width: 96px;"
          src="user137.PNG"
          alt="user137 Profile Picture"
        />
      </a>
    </div>
    <div>
      <a
        style="margin: 30px;
      padding: 12px;
      border-radius: 15px;
      border: 6px solid rgba(10, 202, 166, 0.9);
      
      width: auto;"
        href="https://auditutils.com/"
      >
        <img
          style="max-width: 96px;"
          src="https://auditutils.com/content/images/2023/02/au-pixelize.jpg"
          alt="auditutils logo pixel"
        />
      </a>
    </div>
  </div>
  <h1>AUtoken faucet</h1>
  <div>
    <img
          style="max-width: 96px;"
          src="au-token-150px-t.png"
          alt="auditutiltoken logo"
        />
  </div>

  <p>
    Obtain your AUtoken by connecting your wallet and submitting a request. The
    receipt of 1000 AUtoken requires payment of gas fees (GETH) only.
  </p>
  <p>
    These tokens may be utilized in demonstration applications on the goerli
    test net:
  </p>
  <button on:click={connectWallet}>Connect Wallet</button>
  <button on:click={requestTokens}>Request Token</button>
  <br />
  <a
    style="font-family: monospace;
  color: #fff;
  font-size: 1.5em;
  width: auto;"
    href="https://goerli.etherscan.io/address/0x226074b8FdBF962370E08948BBd9AFc828c08363"
    target="_blank">View the smart contract on Etherscan Goerli</a
  >
</main>
