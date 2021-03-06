# Dash Sentinel

An all-powerful toolset for Dash.

Sentinel is an autonomous agent for persisting, processing and automating Dash V12.1 governance objects and tasks, and for expanded functions in the upcoming Dash V13 release (Evolution).

Sentinel is implemented as a Python application that binds to a local version 12.1 dashd instance on each Dash V12.1 Masternode.

This guide covers installing Sentinel onto an existing 12.1 Masternode in Ubuntu 14.04 / 16.04.

## Installation

### 1. Install Components

Make sure Python version 2.7.x or above is installed.

    python --version

Install Sentinel and dependencies:

    $ sudo apt-get update
    $ sudo apt-get install python-virtualenv

Make sure the local Dash daemon running is version 12.1 (120100), and is configured as a masternode

    $ dash-cli getinfo | grep '"version"'
    $ dash-cli masternode status | grep '"status"'

### 2. Get / Update Source

This section can be used to install Sentinel source or update it for new releases.

Clone the Sentinel code and install Python dependencies.

    $ git clone https://github.com/nmarley/sentinel.git && cd sentinel
    $ virtualenv ./venv
    $ ./venv/bin/pip install -r requirements.txt

### 3. Set up Cron

Set up a crontab entry to call Sentinel regularly, recommended every 2 minutes, by first opening a crontab editor.

    $ crontab -e

In the crontab editor, add the lines below, replacing '/home/YOURUSERNAME/sentinel' to the path where you cloned sentinel to:

    */2 * * * * cd /home/YOURUSERNAME/sentinel && ./venv/bin/python scripts/crontab.py >/dev/null 2>&1

### 4. Test the Configuration

Test the config by runnings all tests from the sentinel folder you cloned into

    $ ./venv/bin/py.test ./test

With all tests passing and crontab setup, Sentinel will stay in sync with dashd and the installation is complete

## Troubleshooting

If Sentinel cannot communicate with dashd, check that RPC settings are configured in dash.conf...

    $ nano ~/.dashcore/dash.conf

...or if dash.conf is stored elsewhere, set a custom path in sentinel.conf with the dash\_conf parameter:

    $ nano sentinel.conf

To debug Sentinel's sync with dashd, run the crontab sync manually and examine the debug output:

    $ SENTINEL_DEBUG=1 ./venv/bin/python scripts/crontab.py

To run individual Sentinel tests, specify the test from the test folder, e.g.

    $ ./venv/bin/py.test test/test_config.py
    $ ./venv/bin/py.test test/test_jsonrpc.py
    $ ./venv/bin/py.test test/test_models.py

## Contributing

Please follow the [DashCore guidelines for contributing](https://github.com/dashpay/dash/blob/v0.12.1.x/CONTRIBUTING.md).

Specifically:

* [Contributor Workflow](https://github.com/dashpay/dash/blob/v0.12.1.x/CONTRIBUTING.md#contributor-workflow)

    To contribute a patch, the workflow is as follows:

    * Fork repository
    * Create topic branch
    * Commit patches

    In general commits should be atomic and diffs should be easy to read. For this reason do not mix any formatting fixes or code moves with actual code changes.

    Commit messages should be verbose by default, consisting of a short subject line (50 chars max), a blank line and detailed explanatory text as separate paragraph(s); unless the title alone is self-explanatory (like "Corrected typo in main.cpp") then a single title line is sufficient. Commit messages should be helpful to people reading your code in the future, so explain the reasoning for your decisions. Further explanation [here](http://chris.beams.io/posts/git-commit/).

### License

Released under the MIT license, undre the same terms as DashCore itself. See [LICENSE](LICENSE) for more info.
