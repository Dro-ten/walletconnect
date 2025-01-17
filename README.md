HTML
<!DOCTYPE html>
<html>
<head>
  <title>WalletConnect DApp</title>
  <script src="https://unpkg.com/@walletconnect/client@1.x.x/dist/walletconnect.min.js"></script>
  <script src="https://unpkg.com/@walletconnect/qrcode-modal@1.x.x/dist/qrcode-modal.min.js"></script>
  <script src="https://unpkg.com/ethers@5.x.x/dist/ethers.min.js"></script>
</head>
<body>
  <button id="connectButton">Connect Wallet</button>
  <p id="walletAddress"></p>
  <button id="sendTransactionButton" disabled>Send Transaction</button>

  <script>
    const client = new Client({
      clientMeta: {
        name: 'My DApp',
        description: 'My awesome dApp',
        url: 'https://my-dapp.com',
        icons: ['https://my-dapp.com/logo.png'],
      },
    });

    const connectWallet = async () => {
      try {
        const uri = await client.connect();
        QRCodeModal.open(uri);

        client.on('session_update', (session) => {
          const address = session.accounts[0];
          document.getElementById('walletAddress').textContent = address;
          document.getElementById('sendTransactionButton').disabled = false;
        });
      } catch (error) {
        console.error('Error connecting wallet:', error);
        // Handle error, e.g., display an error message
      }
    };

    const sendTransaction = async () => {
      try {
        const provider = new ethers.providers.Web3Provider(client);
        const signer = provider.getSigner();

        const contractAddress = '0xYourContractAddress';
        const contractABI = [/* Your Contract ABI */];

        const contract = new ethers.Contract(contractAddress, contractABI, signer);

        const transaction = await contract.yourFunction(/* arguments */);
        await transaction.wait();
        console.log('Transaction sent:', transaction.hash);
      } catch (error) {
        console.error('Error sending transaction:', error);
        // Handle transaction errors gracefully
      }
    };

    document.getElementById('connectButton').addEventListener('click', connectWallet);
    document.getElementById('sendTransactionButton').addEventListener('click', sendTransaction);
  </script>
</body>
</html>
