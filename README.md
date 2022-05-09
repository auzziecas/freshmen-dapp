﻿# freshmen-dapp



<!DOCTYPE html>
<html lang="en">



<head>
    <style>
        body {
            text-align: center;
            font-family: Arial, Helvetica, sans-serif;
        }

        div {
            width: 20%;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
        }

        button {
            width: 100%;
            margin: 10px 0px 5px 0px;
        }
    </style>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My First dApp</title>
</head>

<body>
    <div>
        <h1>This is my dApp!</h1>
        <p>Here we can set or get the mood:</p>
        <label for="mood">Input Mood:</label> <br />
        <input type="text" id="mood" />

        <button onclick="getMood()">Get Mood</button>
        <button onclick="setMood()">Set Mood</button>

    </div>


    <script src="https://cdn.ethers.io/lib/ethers-5.2.umd.min.js" type="application/javascript"></script>
    </script>
    <script>
        const provider = new ethers.providers.Web3Provider(
            window.ethereum,
            "ropsten"
        );

        const MoodContractAddress = "0x25ba8c56a3695db8926970ec38c52b118bd6d322";
        const MoodContractABI = [
            {
                "inputs": [
                    {
                        "internalType": "string",
                        "name": "_mood",
                        "type": "string"
                    }
                ],
                "name": "setMood",
                "outputs": [],
                "stateMutability": "nonpayable",
                "type": "function"
            },
            {
                "inputs": [],
                "name": "getMood",
                "outputs": [
                    {
                        "internalType": "string",
                        "name": "",
                        "type": "string"
                    }
                ],
                "stateMutability": "view",
                "type": "function"
            }
        ];

        let MoodContract;
        let signer;

        provider.send("eth_requestAccounts", []).then(() => {
            provider.listAccounts().then((accounts) => {
                signer = provider.getSigner(accounts[0]);
                MoodContract = new ethers.Contract(
                    MoodContractAddress,
                    MoodContractABI,
                    signer
                );
            });
        });

        async function getMood() {
            const getMoodPromise = MoodContract.getMood();
            const Mood = await getMoodPromise;
            console.log(Mood);
        }

        async function setMood() {
            const mood = document.getElementById("mood").value;
            const setMoodPromise = MoodContract.setMood(mood);
            await setMoodPromise;
        }


    </script>



</body>

</html>
