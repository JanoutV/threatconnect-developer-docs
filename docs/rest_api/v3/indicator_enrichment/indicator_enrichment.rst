====================
Indicator Enrichment
====================

Enriching threat intelligence data helps remove false positives and delivers actionable intelligence for threat investigations and other security operations. ThreatConnect includes built-in enrichment services that you can use to retrieve data from a third-party enrichment service that a System Administrator has enabled on your instance and for a given Indicator type.

Enrich Indicators with Data From an Enrichment Service
------------------------------------------------------

As of ThreatConnect version 7.1, you can use the v3 API to enrich Indicators with data retrieved from the following third-party enrichment services:

- Shodan®
- VirusTotal™

To enrich Indicators using the v3 API, you must append the ``type`` query parameter to the end of the request URL and specify which enrichment service(s) to use. See the following table for a list of accepted values for the ``type`` query parameter.

.. attention::

    The accepted values for the ``type`` query parameter are case sensitive.

.. list-table::
   :widths: 25 25 50
   :header-rows: 1

   * - Value Name
     - Enrichment Service
     - Notes
   * - ``Shodan``
     - Shodan
     - Available for Address Indicators only
   * - ``VirusTotalV3``
     - VirusTotal
     - Available for Address, File, Host, and URL Indicators only

API requests to enrich an Indicator will use the API key your System Administrator entered when enabling and configuring the specified enrichment service to retrieve data from the enrichment service.

.. attention::

    If the API key your System Administrator entered for an enrichment service exceeds the quota limit set by the enrichment vendor, an error message stating so will be returned by the API.

.. note::

    If you enrich an Indicator that exists in multiple owners, enrichment data will be available for each copy of the Indicator. However, only a single API request will be sent to the specified enrichment service.

Enrich a Specific Indicator
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Send a request in the following format to enrich a specific Indicator with data retrieved from the specified enrichment service:

.. code::

    POST /v3/indicators/{indicatorId or indicatorSummary}/enrich?type={enrichmentService}

For example, the following request will enrich the **71.6.135.131** Address Indicator with data retrieved from Shodan:

.. code::

    POST /v3/indicators/71.6.135.131/enrich?type=Shodan

JSON Response

.. code::

    {
        "data": {
            "id": 15,
            "dateAdded": "2022-09-22T11:47:56Z",
            "ownerId": 1,
            "ownerName": "Demo Organization",
            "webLink": "https://app.threatconnect.com/#/details/indicators/15/overview",
            "type": "Address",
            "lastModified": "2022-09-22T11:47:56Z",
            "summary": "71.6.135.131",
            "privateFlag": false,
            "active": true,
            "activeLocked": false,
            "ip": "71.6.135.131",
            "legacyLink": "https://app.threatconnect.com/auth/indicators/details/address.xhtml?address=71.6.135.131&owner=Demo+Organization",
            "enrichment": {
                "data": [
                    {
                        "type": "Shodan",
                        "hostNames": [
                            "soda.census.224.154.51.115",
                            "soda.census.224.59.169.138"
                        ],
                        "domains": [
                            "138.",
                            "115."
                        ],
                        "country": "United States",
                        "city": "San Diego",
                        "isp": "CariNet, Inc.",
                        "asn": "AS10439",
                        "org": "CariNet, Inc.",
                        "openPorts": [
                            {
                                "transport": "tcp",
                                "port": 22,
                                "product": "OpenSSH",
                                "data": "SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.5\nKey type: ssh-rsa\nKey: AAAAB3NzaC1yc2EAAAADAQABAAABAQCjl6EMm/rwCVDPD0bpSJc5HUfbWxgddKI6L+23g3h+kSNK\nAj4qh+RwT5InvQA6Rqkdc7e0fs+tm1MejA6vkV+7ZX7iKnG00tEi+uM7aEmRZl5CU6O2GNfSYgq9\nzOmhY1ZhRi3OaInZnkDBaYFo1KkGIyzc+ulkW8uch2/WwXuCCC7Yp2IzUdv/pgZgssPqJR0e2Nn/\nub87QA3ayw5V5rEQDq2ESpkEiCUhp8RN4wJAUyEsJMWMV80gOb7obykIc/mtkzjsjh6hvVuPhBGZ\n4govHkmFNNx1hDJ/lRajU006SnJmVZiLwN7yLOmw6F6bqo1qd/REngHRyLvgeuXyfkiN\nFingerprint: 89:8e:ba:1c:71:45:32:41:b4:8a:fe:91:85:3b:16:07\n\nKex Algorithms:\n\tcurve25519-sha256\n\tcurve25519-sha256@libssh.org\n\tecdh-sha2-nistp256\n\tecdh-sha2-nistp384\n\tecdh-sha2-nistp521\n\tdiffie-hellman-group-exchange-sha256\n\tdiffie-hellman-group16-sha512\n\tdiffie-hellman-group18-sha512\n\tdiffie-hellman-group14-sha256\n\tdiffie-hellman-group14-sha1\n\nServer Host Key Algorithms:\n\tssh-rsa\n\trsa-sha2-512\n\trsa-sha2-256\n\tecdsa-sha2-nistp256\n\tssh-ed25519\n\nEncryption Algorithms:\n\tchacha20-poly1305@openssh.com\n\taes128-ctr\n\taes192-ctr\n\taes256-ctr\n\taes128-gcm@openssh.com\n\taes256-gcm@openssh.com\n\nMAC Algorithms:\n\tumac-64-etm@openssh.com\n\tumac-128-etm@openssh.com\n\thmac-sha2-256-etm@openssh.com\n\thmac-sha2-512-etm@openssh.com\n\thmac-sha1-etm@openssh.com\n\tumac-64@openssh.com\n\tumac-128@openssh.com\n\thmac-sha2-256\n\thmac-sha2-512\n\thmac-sha1\n\nCompression Algorithms:\n\tnone\n\tzlib@openssh.com\n"
                            },
                            {
                                "transport": "tcp",
                                "port": 9002,
                                "data": "\\xff\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x01\\x7f"
                            }
                        ]
                    }
                ]
            }
        },
        "status": "Success"
    }

Enrich Multiple Indicators
^^^^^^^^^^^^^^^^^^^^^^^^^^

Send a request in the following format to enrich multiple Indicators with data retrieved from the specified enrichment service:

.. code::

    POST /v3/indicators/enrich?type={enrichmentService}
    {
        "data": [
            {
                "type": "<indicatorType>",
                "summary": "<indicatorSummary>"
            },
            {
                "type": "<indicatorType>",
                "summary": "<indicatorSummary>"
            },
            {...}
        ]
    }

The specified enrichment service must be available for each type of Indicator included in the request body. For example, the following request will enrich the **71.6.135.131** Address Indicator and **nemesis.com** Host Indicator with data retrieved from VirusTotal:

.. code::

    POST /v3/indicators/enrich?type=VirusTotalV3
    {
        "data": [
            {
                "type": "Address",
                "summary": "71.6.135.131"
            },
            {
                "type": "Host",
                "summary": "nemesis.com"
            }
        ]
    }

JSON Response

.. code::

    {
        "enriched": 2,
        "status": "Success"
    }

Enrich Indicators with Multiple Enrichment Services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can specify multiple enrichment services from which to retrieve data when enriching one or more Indicators. In this scenario, each enrichment service must be available for the type(s) of Indicator(s) you want to enrich.

For example, the following request will enrich two Address Indicators with data retrieved from Shodan and VirusTotal:

.. code::

    POST /v3/indicators/enrich?type=Shodan&type=VirusTotalV3
    {
        "data": [
            {
                "type": "Address",
                "summary": "71.6.135.131"
            },
            {
                "type": "Address",
                "summary": "96.38.88.212"
            }
        ]
    }

JSON Response

.. code::

    {
        "enriched": 2,
        "status": "Success"
    }

If an enrichment service is not available for an Indicator type included in the request, then the request will enrich the Indicator types for which the specified enrichment service is available and return a message indicating which Indicators types could not be enriched with that service. For example, the following request attempts to enrich an Address and Host Indicator with data retrieved from Shodan and VirusTotal. Because Shodan is available for Address Indicators only, the API response includes a message stating that the Host Indicator cannot be enriched with Shodan.

.. code::

    POST /v3/indicators/enrich?type=Shodan&type=VirusTotalV3
    {
        "data": [
            {
                "type": "Address",
                "summary": "71.6.135.131"
            },
            {
                "type": "Host",
                "summary": "nemesis.com"
            }
        ]
    }

JSON Response

.. code::

    {
        "enriched": 1,
        "unableEnrich": 1,
        "messages": [
            "The Host nemesis.com cannot be enriched with Shodan because the indicator type isn't supported."
        ],
        "status": "Success"
    }

Include Enrichment Data in API Responses
----------------------------------------

When using the ``/v3/indicators`` endpoint to create, retrieve, or update Indicators, you can use the ``fields`` `query parameter <https://docs.threatconnect.com/en/latest/rest_api/v3/additional_fields.html>`_ to include the ``enrichment`` field in API responses.

Send a request in the following format to retrieve data for all Indicators or a specific one and include enrichment data for the Indicator(s) in the API response:

Request (All Indicators)

.. code::

    GET /v3/indicators?fields=enrichment

Request (Specific Indicator)

.. code::

    GET /v3/indicators/{indicatorId or indicatorSummary}?fields=enrichment

.. attention::

    You must first enrich an Indicator with a supported enrichment service for data to be populated in the ``enrichment`` field included in the API response.

For example, the following request will retrieve data for the **71.6.135.131** Address Indicator and include enrichment data for the Indicator in the API response:

.. code::

    GET /v3/indicators/71.6.135.131?fields=enrichment

JSON Response

.. code::

    {
        "data": {
            "id": 15,
            "dateAdded": "2022-09-22T11:47:56Z",
            "ownerId": 1,
            "ownerName": "Demo Organization",
            "webLink": "https://app.threatconnect.com/#/details/indicators/15/overview",
            "type": "Address",
            "lastModified": "2022-09-22T11:47:56Z",
            "summary": "71.6.135.131",
            "privateFlag": false,
            "active": true,
            "activeLocked": false,
            "ip": "71.6.135.131",
            "legacyLink": "https://app.threatconnect.com/auth/indicators/details/address.xhtml?address=71.6.135.131&owner=Demo+Organization",
            "enrichment": {
                "data": [
                    {
                        "type": "VirusTotal",
                        "vtMaliciousCount": 10,
                        "vtLastUpdated": "2023-03-20T13:00:29Z"
                    },
                    {
                        "type": "Shodan",
                        "hostNames": [
                            "soda.census.224.154.51.115",
                            "soda.census.224.59.169.138"
                        ],
                        "domains": [
                            "138.",
                            "115."
                        ],
                        "country": "United States",
                        "city": "San Diego",
                        "isp": "CariNet, Inc.",
                        "asn": "AS10439",
                        "org": "CariNet, Inc.",
                        "openPorts": [
                            {
                                "transport": "tcp",
                                "port": 22,
                                "product": "OpenSSH",
                                "data": "SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.5\nKey type: ssh-rsa\nKey: AAAAB3NzaC1yc2EAAAADAQABAAABAQCjl6EMm/rwCVDPD0bpSJc5HUfbWxgddKI6L+23g3h+kSNK\nAj4qh+RwT5InvQA6Rqkdc7e0fs+tm1MejA6vkV+7ZX7iKnG00tEi+uM7aEmRZl5CU6O2GNfSYgq9\nzOmhY1ZhRi3OaInZnkDBaYFo1KkGIyzc+ulkW8uch2/WwXuCCC7Yp2IzUdv/pgZgssPqJR0e2Nn/\nub87QA3ayw5V5rEQDq2ESpkEiCUhp8RN4wJAUyEsJMWMV80gOb7obykIc/mtkzjsjh6hvVuPhBGZ\n4govHkmFNNx1hDJ/lRajU006SnJmVZiLwN7yLOmw6F6bqo1qd/REngHRyLvgeuXyfkiN\nFingerprint: 89:8e:ba:1c:71:45:32:41:b4:8a:fe:91:85:3b:16:07\n\nKex Algorithms:\n\tcurve25519-sha256\n\tcurve25519-sha256@libssh.org\n\tecdh-sha2-nistp256\n\tecdh-sha2-nistp384\n\tecdh-sha2-nistp521\n\tdiffie-hellman-group-exchange-sha256\n\tdiffie-hellman-group16-sha512\n\tdiffie-hellman-group18-sha512\n\tdiffie-hellman-group14-sha256\n\tdiffie-hellman-group14-sha1\n\nServer Host Key Algorithms:\n\tssh-rsa\n\trsa-sha2-512\n\trsa-sha2-256\n\tecdsa-sha2-nistp256\n\tssh-ed25519\n\nEncryption Algorithms:\n\tchacha20-poly1305@openssh.com\n\taes128-ctr\n\taes192-ctr\n\taes256-ctr\n\taes128-gcm@openssh.com\n\taes256-gcm@openssh.com\n\nMAC Algorithms:\n\tumac-64-etm@openssh.com\n\tumac-128-etm@openssh.com\n\thmac-sha2-256-etm@openssh.com\n\thmac-sha2-512-etm@openssh.com\n\thmac-sha1-etm@openssh.com\n\tumac-64@openssh.com\n\tumac-128@openssh.com\n\thmac-sha2-256\n\thmac-sha2-512\n\thmac-sha1\n\nCompression Algorithms:\n\tnone\n\tzlib@openssh.com\n"
                            },
                            {
                                "transport": "tcp",
                                "port": 9002,
                                "data": "\\xff\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x01\\x7f"
                            }
                        ]
                    }
                ]
            }
        },
        "status": "Success"
    }

----

*Shodan® is a registered trademark of Shodan.*
*VirusTotal™ is a trademark of Google, Inc.*