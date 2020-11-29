# MongoDB

Official docs: <https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/>

* Start mongoDB:

    ``` shell
    brew services start mongodb-community@4.2
    ```

* Stop mongoDB:

    ``` shell
    brew services stop mongodb-community@4.2
    ```

* Start replica from existing db
    1. Change brew service config

        ``` shell
        /usr/local/Cellar/mongodb-community/4.2.2/homebrew.mxcl.mongodb-community.plist
        ```

        Open the above file by xCode and add program arguments. Add item 4 to '--replSet'. Add item 5 to 'rs'.

    2. Restart brew service mongodb:

        ``` shell
        brew services restart mongodb-community@4.2
        ```

    3. Enter into mongo shell and initiate replication:

        ``` shell
        mongo
        rs.initiate()
        ```
