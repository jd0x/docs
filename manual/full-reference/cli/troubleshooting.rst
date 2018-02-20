Troubleshooting
===============

[1000] Directory is not a Hasura project
----------------------------------------

This error is thrown when a Hasura CLI command is executed in a directory which is not a Hasura project.
Make sure ``clusters.yaml``, ``hasura.yaml``, ``microservices/``, ``migrations/``, ``conf/`` are present and are not empty.
You can check https://github.com/hasura/base for reference.

[1035] Cannot connect to cluster: network call timed out
--------------------------------------------------------

Open http://portquiz.net:3443 from your browser to check if port 3443 is blocked on your network.
If you cannot open the page in a browser, access to port 3443 is blocked on your network (typically seen in corporate/campus networks). 
You can confirm this by opening the URL from a different network like mobile data.
Also check for access to port 22 by visiting  http://portquiz.net:22
CLI needs access to these ports to function. If you cannot access these 2 URLs, Hasura CLI will not work on your network and you need to contact 
