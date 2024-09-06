Starknet's Counter Workshop
In this workshop, you will learn how to create a simple Starknet smart contract, implement public functions, and events, access external contracts, and use OpenZeppelin's Ownable contract.

After completing each step, run the associated script to verify it has been implemented correctly.

Use the Cairo book and the Starknet docs as a reference.

Setup
Clone this repository and choose whether you prefer using Docker to manage global dependencies or not in the following steps:

Option 1: Without Docker
Install asdf (instructions)
Install Scarb 2.8.0 via asdf (instructions)
Install Starknet Foundry 0.27.0 via asdf (instructions)
Install the Cairo 1.0 extension for VSCode (marketplace)
Option 2: With Docker
Make sure Docker is installed and running
Install the Dev Containers extension for VSCode (marketplace)
Launch an instance of VSCode inside of the container by going to View -> Command Palette -> Dev Containers: Rebuild and Reopen in Container
Note: All the commands shown from this point on will assume that you are using the integrated terminal of a VSCode instance running inside the container. If you want to run the tests on a different terminal you'll need to use the command docker compose run test.

Step 1
Switch to the step1 branch to enable the verification tests:

git checkout -b step1 origin/step1
Goal
Initialize the project structure within the cloned repository by using the Scarb package manager and enable compilation of Starknet Contracts.

Requirements
When initializing the project with Scarb, name the package as workshop
Create a new Cairo file under the src directory named counter.cairo, and add the following starting code:
#[starknet::contract]
pub mod counter_contract {
    #[storage]
    struct Storage {}
}
In the lib.cairo file remove the code and define the counter module
Note: Using any other name will disrupt upcoming steps.

Verification
When completed, build your project by running the following command:

scarb build
Hints
Check out the scarb init command to initialize a project. In case you want to initialize the project with a specific name, you can use the scarb init --name PACKAGE_NAME command.
Refer to the Cheat Sheet for essential Scarb commands
To enable Starknet Contract compilation:
Target starknet-contract.
Specify the Cairo version in the Scarb.toml.
Learn more in the Starknet Contract Target documentation.
Step 2
Switch to the step2 branch to enable the verification tests:

git checkout -b step2 origin/step2
Goal
Add snforge as a dependency within your Scarb.toml file to allow execution of tests with Starknet Foundry.

Requirements
In your Scarb.toml, declare the snforge_std package as your project dependency and enable casm contract class generation
In your Scarb.toml, define a script named test to be able to run snforge test command
In your Scarb.toml, set your edition to target 2023_01
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
Specify the version of Starknet Foundry that the project currently uses
Refer to the Starknet Foundry Documention for more information.
Refer to the Scarb Running Scripts Documentation for more information.
edition = "2024_07" is a default configuration from Scarb that targets the July 2024 version of Cairo prelude. However, in our workshop we will work with 2023_01 for simplicity. Refer to the Prelude Documentation for more information.
Step 3
Switch to the step3 branch to enable the verification tests:

git checkout -b step3 origin/step3
Goal
Implement the constructor function to initialize an input number and store a variable named counter within the contract.

Requirements
Store a variable named counter as u32 type in the Storage struct.
Implement the constructor function that initializes the counter variable with a given input value.
The input variable of the constructor function should be named initial_value
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
Storage variables are the most common way to interact with your contract storage. You can read more about it in Chapter 14 - Contract Storage.
The constructor function is a special type of function that runs only once. You can read more about it in Chapter 14 - Constructors.
Step 4
Switch to the step4 branch to enable the verification tests:

git checkout -b step4 origin/step4
Goal
Implement an interface for the contract which contains the get_counter() function. This function should return the value of the stored counter variable within the contract.

Requirements
Implement an interface for a function named get_counter() which returns the value of the counter variable.
The get_counter() function must be within the contract's interface named ICounter.
Note: Any other given name to the contract's interface would break the test, be sure to have to correct name!

Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
To create a contract interface, you will need to define a trait with the name ICounter (otherwise the tests will fail) and mark the trait with the [starknet::interface] attribute. You can read more about it in Chapter 13 Anatomy of a Simple Contract.
The get_counter() function should only be able to read the state of the contract and not modify it. You can read more about it in Chapter 14 - View Functions.
Step 5
Switch to the step5 branch to enable the verification tests:

git checkout -b step5 origin/step5
Goal
Within the same interface created in the previous step, implement a function called increase_counter() that can increment the current value of the counter by 1 each time it is invoked.

Requirements
Implement a function named increase_counter() which increments the counter value by 1.
The increase_counter() function must be within the contract's interface named ICounter.
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
The increase_counter() function should be able to modify the state of the contract (also called an external function) and update the counter value within the Storage. You can read more about it in Chapter 14 - External Functions.
Step 6
Switch to the step6 branch to enable the verification tests:

git checkout -b step6 origin/step6
Goal
Implement an event named CounterIncreased that emits the current value of the counter variable, every time the value is increased.

Requirements
Define a variant named CounterIncreased in the Event enum.
Defining the value variable within the CounterIncrease struct.
Emit the event in the increase_counter() function with the new value, once the counter value has been incremented.
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
Events are custom data structures that are emitted by a contract. More information about Events can be found in Chapter 14 - Contract Events.
To emit an event, you can use the self.emit() function as show here.
Note: CHECKPOINT Reached ⛳️! Switch to the step15-js branch to get a deployment script based on starknet.js.

git checkout -b step15-js origin/step15-js
Step 7
Switch to the step7 branch to enable the verification tests:

git checkout -b step7 origin/step7
Goal
In this step, we will introduce an external smart contract that acts as a kill switch for a specific function. Your task is to add the external KillSwitch contract as a dependency within your project.

Note: The KillSwitch contract can be found here.

Requirements
In your Scarb.toml file, declare the kill_switch package as your project dependency under the [dependencies] section.
In your Scarb.toml file, to allow compilation of external contracts for Starknet Foundry, add the following line under the [[target.starknet-contract]] section.
build-external-contracts = ["kill_switch::KillSwitch"]
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
Refer to the Scarb Managing Dependencies Documention for more information.
Refer to the Compiling External Cotnract for more information.
Step 8
Switch to the step8 branch to enable the verification tests:

git checkout -b step8 origin/step8
Goal
Initialize the KillSwitch contract by storing the contract's address given as an input variable in the constructor function.

Requirements
Store a variable named kill_switch as type ContractAddress.
Update the constructor function to initialize the kill_switch variable.
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
The task is similar to Step 3. Refer to it for more information.
Step 9
Switch to the step9 branch to enable the verification tests:

git checkout -b step9 origin/step9
Goal
Implement the KillSwitch mechanism in the increase_counter() by calling the is_active() function from the KillSwitch contract.

Requirements
If the function is_active() from the KillSwitch contract returns false, then allow the increase_counter() function to increment the value; otherwise, return without incrementing.
Note: Analyze the KillSwitch code to understand the interface and the contract structure from here.

Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
You need to import the Dispatcher and DispatcherTrait of the KillSwitch contract. These dispatchers are automatically created and exported by the compiler. More information about Contract Dispatcher can be found in Chapter 15.2 - Contract Dispatcher.
You can access the is_active() function from your KillSwitch contract dispatcher.
You can use an if expression to implement the mechanism. Refer to the Cairo Book to learn more.
Note: If you want to deploy the Counter contract, you can use the following deployed KillSwitch contract address.

Sepolia
Contract Address: 0x05f7151ea24624e12dde7e1307f9048073196644aa54d74a9c579a257214b542

Voyager
Starkscan
Step 10
Switch to the step10 branch to enable the verification tests:

git checkout -b step10 origin/step10
Goal
Protect the increase_counter() function by reverting the transaction if KillSwitch mechanism is enabled.

Requirements
Create the condition to revert the transaction if the KillSwith contract is enabled
Revert the transaction with the following message Kill Switch is active
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
You can stop and revert a transaction with an error message using the assert!() macro. Refer to the Cairo Book documentation to learn more.
You can replace the if expression with the assert!() macro instead.
Step 11
Switch to the step11 branch to enable the verification tests:

git checkout -b step11 origin/step11
Goal
Add the external OpenZeppelin contracts as a dependency within your project.

Note: The OpenZeppelin contracts can be found here.

Requirements
In your Scarb.toml file, declare the openzeppelin package as your project dependency under the [dependencies] section.
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

$ scarb test
Hints
Specify the OpenZeppelin tag version as v0.16.0 in Scarb.toml.
Refer to the OZ Contracts for Cairo Documention for more information.
Step 12
Switch to the step12 branch to enable the verification tests:

git checkout -b step12 origin/step12
Goal
Initialize the Ownable component from the OpenZeppelin contracts.

Before working on this step, make sure to read Chapter 16.2: Composability and Components and see how Components work.

Requirements
Declare the component inside your contract using the component!() macro.
Add the path to the component's storage and events to the contract's Storage and Event.
Embed the component's logic into your contract by creating an instance of the component's generic implementation with a specific ContractState.
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
Refer to the Using Components Inside a Contract documentation to learn how to implement a component within a contract.
Step 13
Switch to the step13 branch to enable the verification tests:

git checkout -b step13 origin/step13
Goal
Modify the constructor function to call the initializer() function within the Ownable component to initialize the owner.

Requirements
The input variable of the constructor function should be named initial_owner
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hint
To call the initializer() function you can look at what functions self.ownable exposes.
Refer to the Ownable Component to learn more about the accessible function.
Step 14
Switch to the step14 branch to enable the verification tests :

git checkout -b step14 origin/step14
Goal
Protect the increase_counter() function so that only the owner of the contract can call this.

Requirements
Use the assert_only_owner() function from the Ownable Component.
Verification
When completed, execute the test suite to verify you've met all the requirements for this section.

scarb test
Hints
To call the assert_only_owner() function you can look at what functions self.ownable exposes.
Check out the assert_only_owner() function from the Ownable Component for more information.
Step 15
Switch to the step15-js branch to get a deployment script based on starknet.js.

git checkout -b step15-js origin/step15-js
Goal
To deploy your account contract to Starknet's testnet using the deploy.ts script found in the scripts folder.

Dependencies
Run the command below from the project's root folder to install the deployment script dependencies.

npm install
Deployer Wallet
Create a wallet that the script can use to pay for the declaration of your account contract.

Steps
Create a wallet on Starknet testnet using the Argent X or Braavos browser extension.
Fund the wallet by using the Faucet or the Bridge.
Create a file in the project's root folder called .env
Export the private key of the funded wallet and paste it into the .env file using the key DEPLOYER_PRIVATE_KEY.
DEPLOYER_PRIVATE_KEY=<YOUR_FUNDED_TESTNET_WALLET_PRIVATE_KEY>
Export the public key of the funded wallet and paste it into the .env file using the key DEPLOYER_ADDRESS
DEPLOYER_ADDRESS=<YOUR_FUNDED_TESTNET_WALLET_PUBLIC_ADDRESS>
RPC Endpoint
To successfully deploy the contract with the script on the Starknet Testnet, you will need to provide an RPC URL. For our workshop, we will use Blast's Public RPC Endpoint.

Add the following line in your .env file:

RPC_ENDPOINT=https://starknet-sepolia.public.blastapi.io/
Refer to Blast to learn more about their Starknet RPC Endpoints.

Run the Script
Run the script that will declare and deploy your smart contract on the Starknet Testnet and ensure that you adjust the constructor inputs in your deploy.ts file appropriately.

Note: If you are deploying the smart contract from the CHECKPOINT, in the deploy.ts file, you will only need the initial_counter in the constructor variable. Ensure that you remove or comment out the kill_switch_address and initial_onwer variables.

Additionally, ensure that the variable names in the deploy.ts constructor are the same as in the counter.cairo constructor function.

const constructor = myCallData.compile("constructor", {
  initial_counter: 100,
  address: "0x05f7151ea24624e12dde7e1307f9048073196644aa54d74a9c579a257214b542",
  initial_owner: process.env.DEPLOYER_ADDRESS,
});
Steps
From the project's root folder run npm run deploy
Follow the instructions from the terminal
If the script finishes successfully your smart contract is ready to be used on Starknet testnet. Congratulations!
