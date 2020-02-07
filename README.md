# How to run Hashserve tests

This set of tests leverages Postman's Collection Runner to allow running groups of tests easily as well as share the tests. It also has some nice functionality like allowing the same test to be run against an entire collection without needing to duplicate it on every request, plus plenty of testing libraries. 

The tests and supporting data can be pulled from this repo and imported into Postman. If you don't want to install Postman, I've exported the test results and test runs, which are in json format.

Requirements:
- Postman Installed. I was using Postman Version 7.17.0. on Mac.
- The "hashserve.postman_environment.json" file set as the environment in Postman. The port used to run the hashserve service is port 2000, but feel free to update the environment variable to reflect your current port. 
- There are three test runs that can be directly imported into the Runner and executed. Two require data files, see the "Running Tests" section.
- The test collection is "hashserve.postman_environment.json" - To import the collection files in Postman, click the Import button in the header bar. You can then peruse the collection. 

Tests:
Tests that apply to every request in a folder are written on the folder. They are reported out in a test run and can be viewed by clicking the action menu on the folder, selecting Edit, then Tests.
Tests that apply to a particular request are found on the Tests section of the request. They are also reported in a test run.

Running Tests:
To execute a test run, open the Runner and click Import. Select the run. 
Ensure the hashserve service is running and your port is exported and matches the port variable in your Postman environment.
- The "Happy-Path-And-Negative-Run.json" only needs the environment selected. *** NOTE *** The last test will shut down the hashserve service. You must restart it manually to continue running tests.
- The "Positive-Password-Run.json" requires the environment to be set AND the "positive-password-list.csv" selected as the "Data File"
- The "Negative-Password-Run.json" requires the environment to be set AND the "negative-password-list.csv" selected as the "Data File" *** NOTE *** These tests fail -- see bug report "Bug: Password parameter and value should be required fields"

Manual Tests:
I executed a couple tests manually so I could time box my efforts here. 
I tested that the service finishes processing an in-flight hash, but rejects incoming requests received after the shutdown signal. This passed.
I tested the special characters "," and backslash manually because they really didn't play nicely with Postman's data file. The backslash failed (see bug report) and the comma...passed...but I swear it failed initially. Did you include a Heisenbug? 

Areas for Improvement:
I didn't test double or multi-byte characters. Again, timebox. 
I'm not super happy with how the tests are organized and that you can't run all of them by pushing one button. (But the data files were the more maintainable route.) I also looked into whether it would be possible to start the hashserver from Postman, and the answer was "probably" (insert three hoops to jump through) so that would be interesting to look into. The /hash/# test also is dependent on a job id existing, so right now it's dependent on the previous test in the suite. That's a bit of an anti-pattern to have interdependent tests.
