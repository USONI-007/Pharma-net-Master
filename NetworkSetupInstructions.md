## Network Setup Instructions

1. Pre-setup
    1. Generate Crypto Materials
		```console
        Umesh SHARMA >> $ cryptogen generate --config=./crypto-config.yaml
	
	2. Generate Channel Artifacts
	    ```console
        Umesh SHARMA >> $ configtxgen -profile OrdererGenesis -channelID upgrad-sys-channel -outputBlock ./channel-artifacts/genesis.block
        Umesh SHARMA >> $ configtxgen -profile CertificationChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID certificationchannel
	    Umesh SHARMA >> $ configtxgen -profile CertificationChannel -outputAnchorPeersUpdate ./channel-artifacts/iitMSPanchors.tx -channelID certificationchannel -asOrg iitMSP
	    Umesh SHARMA >> $ configtxgen -profile CertificationChannel -outputAnchorPeersUpdate ./channel-artifacts/mhrdMSPanchors.tx -channelID certificationchannel -asOrg mhrdMSP
	    Umesh SHARMA >> $ configtxgen -profile CertificationChannel -outputAnchorPeersUpdate ./channel-artifacts/upgradMSPanchors.tx -channelID certificationchannel -asOrg upgradMSP

2. Docker Network Setup
	1. Start Docker Network
	    ```console
        Umesh SHARMA >> $ docker-compose -f ./docker-compose.yml up -d

3. Fabric Network Setup
	1. SSH Into CLI Container
	    ```console
        Umesh SHARMA >> $ docker exec -it cli /bin/bash
	2. Create Channel
	    ```console
        Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="iitMSP"
		Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/iit.certification-network.com/users/Admin@iit.certification-network.com/msp
		Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.iit.certification-network.com:7051
		Umesh SHARMA >> $ peer channel create -o orderer.certification-network.com:7050 -c certificationchannel -f ./channel-artifacts/channel.tx
	3. Join Peer 0 - IIT
		```console
        Umesh SHARMA >> $ peer channel join -b certificationchannel.block
	4. Join Peer 1 - IIT
        ```console
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer1.iit.certification-network.com:8051
        Umesh SHARMA >> $ peer channel join -b certificationchannel.block
    5. Join Peer 0 - MHRD
        ```console
        Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="mhrdMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/mhrd.certification-network.com/users/Admin@mhrd.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.mhrd.certification-network.com:9051
        Umesh SHARMA >> $ peer channel join -b certificationchannel.block
    6. Join Peer 1 - MHRD
        ```console
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer1.mhrd.certification-network.com:10051
        Umesh SHARMA >> $ peer channel join -b certificationchannel.block
    7. Join Peer 0 - UPGRAD
        ```console
        Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="upgradMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/upgrad.certification-network.com/users/Admin@upgrad.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.upgrad.certification-network.com:11051
        Umesh SHARMA >> $ peer channel join -b certificationchannel.block
    8. Join Peer 1 - UPGRAD
        ```console
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer1.upgrad.certification-network.com:12051
        Umesh SHARMA >> $ peer channel join -b certificationchannel.block
    9. Update Anchor Peer for IIT
        ```console
        Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="iitMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/iit.certification-network.com/users/Admin@iit.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.iit.certification-network.com:7051
        Umesh SHARMA >> $ peer channel update -o orderer.certification-network.com:7050 -c certificationchannel -f ./channel-artifacts/iitMSPanchors.tx
    10. Update Anchor Peer for MHRD
        ```console
        Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="mhrdMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/mhrd.certification-network.com/users/Admin@mhrd.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.mhrd.certification-network.com:9051
        Umesh SHARMA >> $ peer channel update -o orderer.certification-network.com:7050 -c certificationchannel -f ./channel-artifacts/mhrdMSPanchors.tx
    11. Update Anchor Peer for UPGRAD
        ```console
        Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="upgradMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/upgrad.certification-network.com/users/Admin@upgrad.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.upgrad.certification-network.com:11051
        Umesh SHARMA >> $ peer channel update -o orderer.certification-network.com:7050 -c certificationchannel -f ./channel-artifacts/upgradMSPanchors.tx


## Install & Instantiate Chaincode

1. Run Chaincode in Dev Mode
	1. SSH Into Chaincode Container
	    ```console
    	Umesh SHARMA >> $ docker exec -it chaincode /bin/bash
    2. Run Chaincode Node App In Dev Mode
        ```console
        Umesh SHARMA >> $ npm run start-dev

2. Install Chaincode
	1. SSH Into CLI Container
		```console
		Umesh SHARMA >> $ docker exec -it cli /bin/bash
	2. Install Chaincode on Peer 0 - IIT
	    ```console
		Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="iitMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/iit.certification-network.com/users/Admin@iit.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.iit.certification-network.com:7051
        Umesh SHARMA >> $ peer chaincode install -n certnet -v 1.1 -l node -p /opt/gopath/src/github.com/hyperledger/fabric/peer/chaincode/
    3. Install Chaincode on Peer 0 - MHRD
        ```console
        Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="mhrdMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/mhrd.certification-network.com/users/Admin@mhrd.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.mhrd.certification-network.com:9051
        Umesh SHARMA >> $ peer chaincode install -n certnet -v 1.1 -l node -p /opt/gopath/src/github.com/hyperledger/fabric/peer/chaincode/
    4. Install Chaincode on Peer 0 - UPGRAD
        ```console
        Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="upgradMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/upgrad.certification-network.com/users/Admin@upgrad.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.upgrad.certification-network.com:11051
        Umesh SHARMA >> $ peer chaincode install -n certnet -v 1.1 -l node -p /opt/gopath/src/github.com/hyperledger/fabric/peer/chaincode/

3. Instantiate Chaincode
	1. SSH Into CLI Container
		```console
		Umesh SHARMA >> $ docker exec -it cli /bin/bash
	2. Instantiate Chaincode on Channel Using Peer 0 - IIT
		```console
		Umesh SHARMA >> $ CORE_PEER_LOCALMSPID="iitMSP"
        Umesh SHARMA >> $ CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/iit.certification-network.com/users/Admin@iit.certification-network.com/msp
        Umesh SHARMA >> $ CORE_PEER_ADDRESS=peer0.iit.certification-network.com:7051
        Umesh SHARMA >> $ peer chaincode instantiate -o orderer.certification-network.com:7050 -C certificationchannel -n certnet -l node -v 1.1 -c '{"Args":["org.certification-network.certnet:instantiate"]}' -P "OR ('iitMSP.member','mhrdMSP.member','upgradMSP.member')"

4. View Container Logs
	1. Start Peer 0 - IIT Container Logs
	    ```console
		Umesh SHARMA >> $ docker logs -f peer0.iit.certification-network.com

5. Test Chaincode
	1. SSH Into Peer 0 - IIT
	    ```console
		Umesh SHARMA >> $ docker exec -it peer0.iit.certification-network.com /bin/bash
	2. Invoke Create Student Function
	    ```console
		Umesh SHARMA >> $ peer chaincode invoke -o orderer.certification-network.com:7050 -C certificationchannel -n certnet -c '{"Args":["org.certification-network.certnet:createStudent","0001","Aakash Bansal","connect@aakashbansal.com"]}'
	3. Invoke Get Student Function
	    ```console
		Umesh SHARMA >> $ peer chaincode invoke -o orderer.certification-network.com:7050 -C certificationchannel -n certnet -c '{"Args":["org.certification-network.certnet:getStudent","0001"]}'
