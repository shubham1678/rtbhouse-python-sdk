RTB House SDK
=============

Overview
--------

This library provides an easy-to-use Python interface to RTB House API. It allows you to read and manage you campaigns settings, browse offers, download statistics etc.


Installation
------------

RTB House SDK can be installed with `pip <https://pip.pypa.io/>`_: ::

    $ pip install rtbhouse_sdk


Usage example
-------------

Let's write a script which fetches campaign stats (imps, clicks, postclicks) and shows the result as a table (using ``tabulate`` library).

First, create ``config.py`` file with your credentials: ::

    USERNAME = 'jdoe'
    PASSWORD = 'abcd1234'


Set up virtualenv and install requirements: ::

    $ pip install rtbhouse_sdk tabulate


.. code-block:: python

    from operator import itemgetter

    from tabulate import tabulate

    from config import USERNAME, PASSWORD
    from rtbhouse_sdk.reports_api import ReportsApiSession

    if __name__ == '__main__':
        api = ReportsApiSession(USERNAME, PASSWORD)
        advertisers = api.get_advertisers()
        day_to = date.today()
        day_from = day_to - timedelta(days=30)
        group_by = ['day']
        metrics = ['impsCount', 'clicksCount', 'campaignCost', 'conversionsCount', 'conversionsValue', 'cr', 'ctr', 'ecpa']
        stats = api.get_rtb_stats(advertisers[0]['hash'], day_from, day_to, group_by, metrics, count_convention=Conversions.ATTRIBUTED_POST_CLICK)
        columns = group_by + metrics
        data_frame = [[row[c] for c in columns] for row in reversed(sorted(stats, key=itemgetter('day')))]
        print(tabulate(data_frame, headers=columns))


License
-------

`MIT <http://opensource.org/licenses/MIT/>`_
