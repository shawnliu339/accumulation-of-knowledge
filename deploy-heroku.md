# Heroku-Deploy

## 1. Deploy to Heroku

1. Create app

    ``` bash
    heroku create <プロジェクト名>
    ```

    Access to `https://<プロジェクト名>.herokuapp.com/` to check

2. Herokuリモートリポジトリを設定します。

    ``` bash
    heroku git:remote --app <プロジェクト名>
    ```

3. Herokuのリモートリポジトリにpush

    ``` bash
    git push heroku master
    ```

## 2. Change deploy command

Heroku uses Procfile to configure deploy command.

Please see here: <https://devcenter.heroku.com/articles/deploying-spring-boot-apps-to-heroku#customizing-the-boot-command>

## 3. Make free plan not sleep

Use New Relic add-on: <https://blog.seiyamaeda.com/9207>
