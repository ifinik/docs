# Подключить репозиторий

{% note warning %}

Подключать репозитории может только [администратор](../role-model.md).

{% endnote %}

Подключите ваши репозитории к {{ tracker-name }}, чтобы привязывать коммиты к задачам. Вы можете подключить любое количество репозиториев, размещенных в сервисах GitHub, GitLab и Bitbucket.

{% note warning %}

К {{ tracker-name }} нельзя подключить Git-репозитории, размещенные на вашем собственном сервере.

{% endnote %}

## GitHub {#section_mhj_yrk_m2b}

Чтобы подключить репозиторий на [GitHub](https://github.com) к {{ tracker-name }}:

{% list tabs %}

- Публичный репозиторий

    1. Убедитесь, что вы авторизованы в {{ tracker-name }} как [администратор](../role-model.md).

    1. На верхней панели {{ tracker-name }} выберите  ![](../../_assets/tracker/icon-settings.png) → **Настроить {{ tracker-name }}**.

    1. Перейдите на вкладку [Репозитории]({{ link-repositories }}) и нажмите кнопку **Подключить репозиторий**.

    1. В поле **Платформа** выберите GitHub.

    1. Укажите адрес публичного репозитория в формате `https://github.com/<логин владельца репозитория>/<имя репозитория>`. Поле **Токен** оставьте пустым.

    1. Нажмите кнопку **Подключить**.

    1. Убедитесь, что статус репозитория отображается в {{ tracker-name }} как **Подключен**.

- Закрытый репозиторий

    1. Убедитесь, что вы авторизованы в {{ tracker-name }} как [администратор](../role-model.md).

    1. На верхней панели {{ tracker-name }} выберите  ![](../../_assets/tracker/icon-settings.png) → **Настроить {{ tracker-name }}**.

    1. Перейдите на вкладку [Репозитории]({{ link-repositories }}) и нажмите кнопку **Подключить репозиторий**.

    1. В поле **Платформа** выберите GitHub.

    1. Получите на GitHub OAuth-токен для своего репозитория:

        - Авторизуйтесь на GitHub. Если репозиторий прикреплен к GitHub-организации, убедитесь, что у вас есть права администратора.

        - Нажмите на свое фото в правом верхнем углу и выберите **Settings**.
        - На панели слева выберите **Developer settings** → **Personal access token**.
        - Нажмите кнопку **Generate new token**.

        - Придумайте название для токена.

        - Настройте разрешения для токена. Для правильной работы с {{ tracker-name }} необходимо, чтобы опции **repo** и **read:org** были включены:

            ![](../../_assets/tracker/github-token-settings.png)

        - Нажмите кнопку **Generate token**.

        - Скопируйте токен в буфер обмена.

            {% note alert %}

            Значение токена отображается только один раз. Если закрыть страницу, вы больше не сможете его просмотреть.

            {% endnote %}

    1. Укажите адрес репозитория в формате `https://github.com/<логин владельца репозитория>/<имя репозитория>` и токен для подключения.

    1. Нажмите кнопку **Подключить**.

    1. Убедитесь, что статус репозитория отображается в {{ tracker-name }} как **Подключен**.

{% endlist %}

Чтобы привязать коммит к задаче в {{ tracker-name }}, укажите ключ задачи в комментарии к коммиту. Привязанные коммиты можно просмотреть:

- на странице задачи на вкладке **Коммиты**;
- на странице очереди на вкладке **Коммиты**.

Если вы не видите вкладки **Коммиты**, убедитесь, что она включена в [настройках очереди](edit-queue-general.md#ul_gcz_xlp_m2b).

## GitLab {#section_kjw_cy2_ggb}

Чтобы подключить репозиторий на [GitLab](https://gitlab.com) к {{ tracker-name }}:

{% list tabs %}

- Публичный репозиторий

    1. Убедитесь, что вы авторизованы в {{ tracker-name }} как [администратор](../role-model.md).

    1. На верхней панели {{ tracker-name }} выберите  ![](../../_assets/tracker/icon-settings.png) → **Настроить {{ tracker-name }}**.

    1. Перейдите на вкладку [Репозитории]({{ link-repositories }}) и нажмите кнопку **Подключить репозиторий**.

    1. В поле **Платформа** выберите GitLab.

    1. Укажите адрес публичного репозитория в формате `https://gitlab.com/<логин владельца репозитория>/<имя репозитория>`. Поле **Токен** оставьте пустым.

    1. Нажмите кнопку **Подключить**.

    1. Убедитесь, что статус репозитория отображается в {{ tracker-name }} как **Подключен**.

- Закрытый репозиторий

    {% note info %}

    Закрытый репозиторий доступен только авторизованным пользователям GitLab (**Internal project**) или команде проекта (**Private project**).

    {% endnote %}

    1. Убедитесь, что вы авторизованы в {{ tracker-name }} как [администратор](../role-model.md).

    1. На верхней панели {{ tracker-name }} выберите ![](../../_assets/tracker/icon-settings.png) → **Настроить {{ tracker-name }}**.

    1. Перейдите на вкладку [Репозитории]({{ link-repositories }}) и нажмите кнопку **Подключить репозиторий**.

    1. В поле **Платформа** выберите GitLab.

    1. Получите на GitLab OAuth-токен для своего репозитория:

        - Авторизуйтесь на GitLab.

        - Нажмите на свое фото в правом верхнем углу и выберите **Settings**.

        - На панели слева выберите **Access Tokens**.

        - Введите название токена в поле **Name**.

        - Задайте срок действия токена в поле **Expires at**.

        - Настройте разрешение для токена. Для правильной работы с {{ tracker-name }} необходимо, чтобы опция **api** была включена:

            ![](../../_assets/tracker/gitlab-token-settings.png)

        - Нажмите кнопку **Create personal access token**.

        - Скопируйте токен в буфер обмена.

            {% note alert %}

            Значение токена отображается только один раз. Если закрыть страницу, вы больше не сможете его просмотреть.

            {% endnote %}

    1. Укажите адрес публичного репозитория в формате `https://gitlab.com/<логин владельца репозитория>/<имя репозитория>` и токен для подключения.

    1. Нажмите кнопку **Подключить**.

    1. Убедитесь, что статус репозитория отображается в {{ tracker-name }} как **Подключен**.

{% endlist %}

Чтобы привязать коммит к задаче в {{ tracker-name }}, укажите ключ задачи в комментарии к коммиту. Привязанные коммиты можно просмотреть:

- на странице задачи на вкладке **Коммиты**;
- на странице очереди на вкладке **Коммиты**.

Если вы не видите вкладки **Коммиты**, убедитесь, что она включена в [настройках очереди](edit-queue-general.md#ul_gcz_xlp_m2b).

## Bitbucket {#section_u5z_zxk_m2b}

Чтобы подключить репозиторий на [Bitbucket](https://bitbucket.org) к {{ tracker-name }}:

1. Убедитесь, что вы авторизованы в {{ tracker-name }} как [администратор](../role-model.md).

1. На верхней панели {{ tracker-name }} выберите ![](../../_assets/tracker/icon-settings.png) → **Настроить {{ tracker-name }}**.

1. Перейдите на вкладку [Репозитории]({{ link-repositories }}) и нажмите кнопку **Подключить репозиторий**.

1. В поле **Платформа** выберите Bitbucket.

1. Создайте для {{ tracker-name }} пароль на Bitbucket. С помощью этого пароля {{ tracker-name }} сможет подключаться к вашим репозиториям.
    - Авторизуйтесь на Bitbucket.
    - Нажмите на свое фото в левом нижнем углу и выберите **Bitbucket settings**.
    - Выберите **App passwords** в блоке **Access management**.
    - Нажмите **Create app password**.
    - Придумайте название для пароля, например `Tracker integration password`.
    - Настройте разрешения для пароля. Для правильной работы с {{ tracker-name }} необходимо, чтобы опция **read** в блоке **Repositories** была включена:
        ![](../../_assets/tracker/bitbucket-password-settings.png)
    - Нажмите кнопку **Create**.
    - Скопируйте пароль в буфер обмена.

        {% note alert %}

        Значение пароля отображается только один раз. Если закрыть страницу, вы больше не сможете его просмотреть.

        {% endnote %}

1. Укажите адрес репозитория в формате `https://bitbucket.org/<логин владельца репозитория>/<имя репозитория>` и ваш логин на Bitbucket.

1. Вставьте пароль, который вы создали для доступа {{ tracker-name }} к Bitbucket. Ваш личный пароль от Bitbucket для этого не подойдет.

1. Нажмите кнопку **Подключить**.

1. Убедитесь, что статус репозитория отображается в {{ tracker-name }} как **Подключен**.

Чтобы привязать коммит к задаче в {{ tracker-name }}, укажите ключ задачи в комментарии к коммиту. Привязанные коммиты можно просмотреть:

- на странице задачи на вкладке **Коммиты**;
- на странице очереди на вкладке **Коммиты**.

Если вы не видите вкладки **Коммиты**, убедитесь, что она включена в [настройках очереди](edit-queue-general.md#ul_gcz_xlp_m2b).

