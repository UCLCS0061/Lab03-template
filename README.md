![GitHub Classroom Workflow](../../workflows/GitHub%20Classroom%20Workflow/badge.svg?branch=main)
<img alt="points bar" align="right" height="36" src="../../blob/badges/.github/badges/points-bar.svg" />

# COMP0061 -- Privacy Enhancing Technologies -- Lab 03

This lab will introduce the basics of Privacy Friendly Computations through Additive Homomorphic Encryption.

### Structure of Labs
The structure of all the labs will be similar: two Python files will be provided. 

- The first is named `Lab0XCode.py` and contains the structure of the code you need to complete. 
- The second is named `Lab0XTests.py` and contains unit tests (written for the Pytest library) that you may execute to 
partially check your answers. 

Note that the tests passing is a necessary but not sufficient condition to fulfill each task.
There are programs that would make the tests pass that would still be invalid (or blatantly insecure) implementations.

The only dependency your Python code should have, besides Pytest and the standard library, is the Petlib library, 
which was specifically developed for this course (and also for our own use!). 

The petlib documentation is [available on-line here](http://petlib.readthedocs.org/en/latest/index.html).

### Checking out code

Check out the code by using your preferred git client (e.g., git command line client, Github Desktop, Sourcetree).
Alternatively, if you are using VSCode, click the `Open in Visual Studio Code` button above to automatically check
out and open the repository.

### Setup
The intended environment for this lab is the Linux operating system with Python 3 installed.

We provide a `setup.sh` file that creates a local virtual environment, installs the dependencies needed for the lab,
and activates the virtual environment. To run the setup file, type `source setup.sh` into the terminal. The virtual
environment is needed to run the unit tests locally. 

*Alternatively:* The tests are the same as the ones that run as part of the Github Classroom automated marking system, 
so you can also run the tests by simply committing and pushing your changes to Github, without the need for a local 
setup or even having Python 3 installed.

### Working with unit tests
Unit tests are run from the command line by executing the command:

```sh
$ pytest -v Lab03Tests.py
```

Note the `-v` flag toggles a more verbose output.
If you wish to inspect the output of the full tests run you may pipe this command to the `less` utility 
(execute `man less` for a full manual of the less utility):

```sh
$ pytest -v Lab03Tests.py | less
```

You can also run a selection of tests associated with each task by adding the Pytest marker for each task to the Pytest
command:

```sh
$ pytest -v Lab03Tests.py -m task1
```
The markers are defined in the test file and listed in `pytest.ini`.

You may also select tests to run based on their name using the `-k` flag.
Have a look at the test file to find out the function names of each test.
For example the following command executes the very first test of Lab 1:

```sh
$ pytest -v Lab03Tests.py -k test_petlib_present
```

The full documentation of pytest is [available here](http://pytest.org/latest/).


### What you will have to submit
The deadline for all labs is at the end of term but labs will be progressively released throughout the term, as new
concepts are introduced. 
We encourage you to attempt labs as soon as they are made available and to use the dedicated lab time to bring up any
queries with the TAs.

Labs will be checked using Github Classroom, and the tests will be run each
time you push any changes to the `main` branch of your Github repository.
The latest score from automarking should be shown in the Readme file.
To see the test runs, look at the Actions tab in your Github repository. 

Make sure the submitted `Lab03Code.py` file at least satisfies the tests, without the need for any external dependency 
except the Python standard libraries and the Petlib library. 
Only submissions prior to the Github Classroom deadline will be marked, so make sure you push your code in time.


To re-iterate, the tests passing is a necessary but not sufficient condition to fulfill each task.
All submissions will be checked by TAs for correctness and your final marks are based on their assessment of your work.  
For full marks, make sure you have fully filled in any sections marked with `TODO` comments, including answering any
questions in the comments of the `Lab03Code.py`.


## TASK 1 -- Additively Homomorphic Encryption \[1 Point\]

> Implement the key generation, encryption and decryption procedures for an additively homomorphic encryption scheme.

## Hints:

- You can run the tests just for this task by executing:
```sh
$ pytest -v Lab03Tests.py -m task1
```

- The encryption scheme is the one described in the lectures on private computations.

- Key generation selects a private key between 1 and the order of the group; the public keys is `x * g`, where is a generator.

- A ciphertext is composed of two elements: `(k *g, k * pub + m * h)`, where `k` is a random number mod the order of the group.

- Do use the table based discrete logarithm function to help implement the decryption operation.

- The `isCiphertext` function should return True for valid ciphertexts.

## TASK 2 -- Define homomorphic operations on ciphertexts \[1 Point\]

> Implement addition and multiplication by a constant over encrypted data.

## Hints:

- You can run the tests just for this task by executing:
```sh
$ pytest -v Lab03Tests.py -m task2
```

- The objective of this task is to perform operations on ciphertext(s) without the knowledge of the secret keys in order to generate new ciphertexts that are functions of the original ones.

- The `add` function takes two ciphertexts and should return a ciphertext of the sum of their plaintexts.

- The `mul` function takes a single ciphertext, and returns a fresh ciphertext encrypting a multiple of its plaintext by a constant `alpha`.

- Both operations return a ciphertext.

## TASK 3 -- Threshold decryption \[1 Point\]

> Define key derivation and partial decryption to facilitate threshold decryption.

## Hints:

- The `GroupKey` operation aggregates a list of public keys from a number of authorities, without using any private keys, to generate a group public key. Encryption under this group key requires all authorities to help with decryption.

- The `partialDecrypt` function takes a ciphertext encypted under a group public key, and returns a partially decrypted ciphertext. 

- The `final` flag signifies that an authority is the last in a decryption chain, and should return a plaintext rather than a partially decrypted ciphertext.

## TASK 4 -- Corrupt threshold decryption authority \[1 Point\]

> Simulate the operation of a corrupt decryption authority.

## Hints:

- The objective of the function `corruptPubKey` is to return a public key that, when combines with other authority keys provided, returns a group key that the corrupt authority can decrypt on its own.

- The private key that should decrypt the corrupted group key is provided as an input to the function.

- Have a look at the tests to see an example of the corrupt authority code in action!

## TASK 5 -- A simple polling example \[1 Point\]

> Implement operations to support a simple private poll.

## Hints:

- The `encode_vote` procedure takes a vote (0 or 1) and returns a pair of ciphertexts representing whether it is a vote for 0 and whether it is a vote for 1.

- The `process_votes` takes a number of individual pairs of votes and returns a pairs of ciphertexts encrypting the total number of votes to 0 and the total number of votes to 1.

- Look at the function `simulate_poll` as a full example of a simulated distributed private poll.

## TASK Qx -- Answer the questions on the basis of your code. \[1 Point\]

> Please include the answers in the comment section provided, and make sure you code file can run correctly.
