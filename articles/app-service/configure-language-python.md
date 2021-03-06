---
title: 配置 Linux Python 应用
description: 了解如何使用 Azure 门户和 Azure CLI 配置运行 Web 应用的 Python 容器。
ms.topic: quickstart
ms.date: 11/06/2020
ms.reviewer: astay; kraigb
ms.custom: mvc, seodec18, devx-track-python, devx-track-azurecli
ms.openlocfilehash: 9e0e9098959231d4283608e8191081ae2df6737a
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94425909"
---
# <a name="configure-a-linux-python-app-for-azure-app-service"></a>为 Azure 应用服务配置 Linux Python 应用

本文介绍 [Azure 应用服务](overview.md)如何运行 Python 应用，以及如何按需自定义应用服务的行为。 必须连同所有必需的 [pip](https://pypi.org/project/pip/) 模块一起部署 Python 应用。

部署 [Git 存储库](deploy-local-git.md)或 [zip 包](deploy-zip.md)时，应用服务部署引擎会自动激活虚拟环境并运行 `pip install -r requirements.txt`。

对于在应用服务中使用内置 Linux 容器的 Python 开发人员，本指南为其提供了关键概念和说明。 若从未使用过 Azure 应用服务，请先按照 [Python 快速入门](quickstart-python.md)以及[将 Python 与 PostgreSQL 配合使用教程](tutorial-python-postgresql-app.md)进行操作。

可使用 [Azure 门户](https://portal.azure.com) 或 Azure CLI 进行配置：

- **Azure 门户**：按照 [在 Azure 门户配置应用服务应用](configure-common.md)中所述，使用应用的“设置” > 配置”页 。

- **Azure CLI**：有两个选项。

    - 在 [Azure Cloud Shell](../cloud-shell/overview.md) 中运行命令。
    - 通过安装最新版的 [Azure CLI](/cli/azure/install-azure-cli) 在本地运行命令，然后使用 [az login](/cli/azure/reference-index#az-login) 登录到 Azure。
    
> [!NOTE]
> Linux 是目前在应用服务中用于运行 Python 应用的推荐选项。 有关 Windows 选项的信息，请参阅 [Windows 风格的应用服务上的 Python](/visualstudio/python/managing-python-on-azure-app-service)。

## <a name="configure-python-version"></a>配置 Python 版本

- **Azure 门户**：按照针对 Linux 容器的 [配置常规设置](configure-common.md#configure-general-settings)中所述，使用“配置”页上的“常规设置”选项卡 。

- **Azure CLI**：

    -  使用 [az webapp config show](/cli/azure/webapp/config#az_webapp_config_show) 显示当前 Python 版本：
    
        ```azurecli
        az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
        ```
        
        将 `<resource-group-name>` 和 `<app-name>` 替换为适合 Web 应用的名称。
    
    - 使用 [az webapp config set](/cli/azure/webapp/config#az_webapp_config_set) 设置 Python 版本
        
        ```azurecli
        az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "PYTHON|3.7"
        ```
    
    - 使用 [az webapp list-runtimes](/cli/azure/webapp#az_webapp_list_runtimes) 显示 Azure 应用服务中支持的所有 Python 版本：
    
        ```azurecli
        az webapp list-runtimes --linux | grep PYTHON
        ```
    
生成自己的容器映像可以运行不受支持的 Python 版本。 有关详细信息，请参阅[使用自定义 Docker 映像](tutorial-custom-container.md?pivots=container-linux)。

<!-- <a> element here to preserve external links-->
<a name="access-environment-variables"></a>

## <a name="customize-build-automation"></a>自定义生成自动化

在使用 Git 或 zip 包部署应用时，应用服务的生成系统（称为 Oryx）执行以下步骤：

1. 如果由 `PRE_BUILD_COMMAND` 设置指定，请运行预先生成的自定义脚本。
1. 运行 `pip install -r requirements.txt`。 requirements.txt 文件必须存在于项目的根文件夹中。 否则，生成过程将报告错误：“找不到 setup.py 或 requirements.txt；未运行 pip install。”
1. 如果在存储库的根文件夹中找到了 manage.py（指示 Django 应用），则运行 manage.py collectstatic 。 但如果 `DISABLE_COLLECTSTATIC` 设置为 `true`，则跳过此步骤。
1. 如果由 `POST_BUILD_COMMAND` 设置指定，请运行后期生成的自定义脚本。

默认情况下，`PRE_BUILD_COMMAND`、`POST_BUILD_COMMAND` 和 `DISABLE_COLLECTSTATIC` 设置为空。 

- 若要在生成 Django 应用时禁用正在运行的 collectstatic，请将 `DISABLE_COLLECTSTATIC` 设置设置为 true。

- 若要运行预先生成的命令，请将 `PRE_BUILD_COMMAND` 设置设置为包含命令（如 `echo Pre-build command`），或包含与项目根文件夹相对的脚本文件的路径（如 `scripts/prebuild.sh`）。 所有命令都必须使用项目根文件夹的相对路径。

- 若要运行后期生成的命令，请将 `POST_BUILD_COMMAND` 设置设置为包含命令（如 `echo Post-build command`），或包含与项目根文件夹相对的脚本文件的路径（如 `scripts/postbuild.sh`）。 所有命令都必须使用项目根文件夹的相对路径。

有关自定义生成自动化的其他设置，请参阅 [Oryx 配置](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md)。 

若要访问生成和部署日志，请参阅[访问部署日志](#access-deployment-logs)。

若要详细了解应用服务如何在 Linux 中运行和生成 Python 应用，请参阅 [Oryx 如何检测和生成 Python 应用](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/python.md)。

> [!NOTE]
> `PRE_BUILD_SCRIPT_PATH` 和 `POST_BUILD_SCRIPT_PATH` 设置与 `PRE_BUILD_COMMAND` 和 `POST_BUILD_COMMAND` 相同，并且支持用于旧用途。
> 
> 如果名为 `SCM_DO_BUILD_DURING_DEPLOYMENT` 的设置包含 `true` 或 1，该设置会在部署过程中触发 Oryx 生成。 当使用 git、Azure CLI 命令 `az webapp up` 和 Visual Studio Code 进行部署时，此设置为 true。

> [!NOTE]
> 始终在所有预先生成和后期生成脚本中使用相对路径，因为运行 Oryx 的生成容器与运行应用的运行时容器不同。 决不要依赖于应用项目文件夹在容器中的确切位置（例如，其位于 site/wwwroot 下）。

## <a name="production-settings-for-django-apps"></a>Django 应用的生产设置

对于 Azure 应用服务之类的生产环境，Django 应用应遵循 Django 的[部署清单](https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/) (djangoproject.com)。

下表描述了与 Azure 相关的生产设置。 这些设置在应用的 setting.py 文件中定义。

| Django 设置 | Azure 说明 |
| --- | --- |
| `SECRET_KEY` | 如[访问作为环境变量的应用设置](#access-app-settings-as-environment-variables)所述，请将值存储在应用服务设置中。 也可以[在 Azure Key Vault 中将该值存储为“机密”](/azure/key-vault/secrets/quick-create-python)。 |
| `DEBUG` | 在应用服务上创建一个值为 0 (false) 的 `DEBUG` 设置，然后将该值加载为环境变量。 在开发环境中，创建一个值为 1 (true) 的 `DEBUG` 环境变量。 |
| `ALLOWED_HOSTS` | 在生产环境中，Django 要求在 settings.py 的 `ALLOWED_HOSTS` 数组中包含应用的 URL。 可使用 `os.environ['WEBSITE_HOSTNAME']` 代码在运行时检索此 URL。 应用服务会自动将 `WEBSITE_HOSTNAME` 环境变量设置为应用的 URL。 |
| `DATABASES` | 在应用服务中为数据库连接定义设置，并将这些设置加载为环境变量以填充 [`DATABASES`](https://docs.djangoproject.com/en/3.1/ref/settings/#std:setting-DATABASES) 字典。 也可以将值（尤其是用户名和密码）存储为 [Azure Key Vault 机密](/azure/key-vault/secrets/quick-create-python)。 |

## <a name="container-characteristics"></a>容器特征

部署到应用服务时，Python 应用在[应用服务 Python GitHub 存储库](https://github.com/Azure-App-Service/python)中定义的 Linux Docker 容器内运行。 可以在特定于版本的目录中找到映像配置。

此容器具有以下特征：

- 应用是结合附加参数 `--bind=0.0.0.0 --timeout 600`，使用 [Gunicorn WSGI HTTP Server](https://gunicorn.org/) 运行的。
    - 如 [Gunicorn 配置概述](https://docs.gunicorn.org/en/stable/configure.html#configuration-file) (docs.gunicorn.org) 中所述，可通过项目根文件夹中的 gunicorn.conf.py 文件为 Gunicorn 提供配置设置。 也可以[自定义启动命令](#customize-startup-command)。

    - 若要保护 Web 应用免遭意外或蓄意的 DDOS 攻击，请按照[部署 Gunicorn](https://docs.gunicorn.org/en/latest/deploy.html) (docs.gunicorn.org) 中所述，Gunicorn 在 Nginx 反向代理之后运行。

- 默认情况下，基础容器映像仅包含 Flask Web 框架，但容器支持符合 WSGI 规范并与 Python 3.6+ 兼容的其他框架，例如 Django。

- 若要安装其他包（例如 Django），请在项目的根文件夹中创建 [requirements.txt](https://pip.pypa.io/en/stable/user_guide/#requirements-files) 文件，该文件指定直接依赖项。 然后，部署项目时，应用服务会自动安装这些依赖项。

    requirements.txt 文件必须位于项目根文件夹中，依赖项才能安装 。 否则，生成过程将报告错误：“找不到 setup.py 或 requirements.txt；未运行 pip install。” 如果遇到此错误，请检查 requirements 文件的位置。

- 应用服务使用 Web 应用的 URL 自动定义名为 `WEBSITE_HOSTNAME` 的环境变量，例如 `msdocs-hello-world.azurewebsites.net`。 它还使用应用的名称定义 `WEBSITE_SITE_NAME`，例如 `msdocs-hello-world`。 
   
## <a name="container-startup-process"></a>容器启动过程

在启动期间，Linux 上的应用服务容器将运行以下步骤：

1. 使用[自定义启动命令](#customize-startup-command)（如果已提供）。
2. 检查是否存在 [Django 应用](#django-app)，如果已检测到，请为其启动 Gunicorn。
3. 检查是否存在 [Flask 应用](#flask-app)，如果已检测到，请为其启动 Gunicorn。
4. 如果未找到其他任何应用，则启动容器中内置的默认应用。

以下部分提供了每个选项的更多详细信息。

### <a name="django-app"></a>Django 应用

对于 Django 应用，应用服务将在应用代码中查找名为 `wsgi.py` 的文件，然后使用以下命令运行 Gunicorn：

```bash
# <module> is the name of the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

若要更精细地控制启动命令，请使用[自定义启动命令](#customize-startup-command)，将 `<module>` 替换为包含 wsgi.py 的文件夹名称，如果该模块不在项目根文件夹中，则添加 `--chdir` 参数。 例如，如果 wsgi.py 位于项目根文件夹中的 knboard/backend/config 下，请使用参数 `--chdir knboard/backend config.wsgi` 。

若要启用生产日志记录，请按照[自定义启动命令](#customize-startup-command)示例中所示，添加 `--access-logfile` 和 `--error-logfile` 参数。

### <a name="flask-app"></a>Flask 应用

对于 Flask，应用服务将查找名为 *application.py* 或 *app.py* 的文件并启动 Gunicorn，如下所示：

```bash
# If application.py
gunicorn --bind=0.0.0.0 --timeout 600 application:app

# If app.py
gunicorn --bind=0.0.0.0 --timeout 600 app:app
```

如果主应用模块包含在不同的文件中，请对应用对象使用不同的名称；若要为 Gunicorn 提供附加的参数，请使用[自定义启动命令](#customize-startup-command)。

### <a name="default-behavior"></a>默认行为

如果应用服务找不到自定义命令、Django 应用或 Flask 应用，它会运行位于 opt/defaultsite 文件夹中的默认只读应用（如下图所示）。

如果部署代码后仍看到默认应用，请参阅[故障排除 - 应用未显示](#app-doesnt-appear)。

[![Linux 上的默认应用服务网页](media/configure-language-python/default-python-app.png)](#app-doesnt-appear)

同样，如果你希望看到已部署的应用，而不是默认应用，请参阅[故障排除 - 应用未显示](#app-doesnt-appear)。

## <a name="customize-startup-command"></a>自定义启动命令

如本文前面所述，可以通过项目根文件夹中的 gunicorn.conf.py 文件为 Gunicorn 提供配置设置，如 [Gunicorn 配置概述](https://docs.gunicorn.org/en/stable/configure.html#configuration-file)中所述。

如果此类配置不够，可以通过在启动命令文件中提供自定义启动命令或多个命令来控制容器的启动行为。 启动命令文件可使用你选择的任何名称，例如 startup.sh、startup.cmd、startup.txt 等  。

所有命令都必须使用项目根文件夹的相对路径。

指定启动命令或命令文件：

- **Azure 门户**：选择应用的“配置”页，然后选择“常规设置” 。 在“启动命令”字段中，输入启动命令的全文或启动命令文件的名称。 然后，选择“保存”，应用所做的更改。 请参阅针对 Linux 容器的[配置常规设置](configure-common.md#configure-general-settings)。

- Azure CLI：使用 [az webapp config set](/cli/azure/webapp/config#az_webapp_config_set) 命令和 `--startup-file` 参数来设置启动命令或文件：

    ```azurecli
    az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
    ```
        
    将 `<custom-command>` 替换为启动命令的全文或启动命令文件的名称。
        
应用服务将忽略处理自定义启动命令或文件时出现的任何错误，然后通过查找 Django 和 Flask 应用来继续执行其启动过程。 如果看不到预期的行为，请检查启动命令或文件是否没有错误，并且启动命令文件是否与应用代码一起部署到应用服务。 还可以检查[诊断日志](#access-diagnostic-logs)以获取更多信息。 还可以在 [Azure 门户](https://portal.azure.com)上查看应用的 **诊断并解决问题** 页面。

### <a name="example-startup-commands"></a>示例启动命令

- **添加了 Gunicorn 参数**：以下示例将 `--workers=4` 添加到用于启动 Django 应用的 Gunicorn 命令行： 

    ```bash
    # <module-path> is the relative path to the folder that contains the module
    # that contains wsgi.py; <module> is the name of the folder containing wsgi.py.
    gunicorn --bind=0.0.0.0 --timeout 600 --workers=4 --chdir <module_path> <module>.wsgi
    ```    

    有关详细信息，请参阅[运行 Gunicorn](https://docs.gunicorn.org/en/stable/run.html) (docs.gunicorn.org)。

- **为 Django 启动生产日志记录：** 将 `--access-logfile '-'` 和 `--error-logfile '-'` 参数添加到命令行：

    ```bash    
    # '-' for the log files means stdout for --access-logfile and stderr for --error-logfile.
    gunicorn --bind=0.0.0.0 --timeout 600 --workers=4 --chdir <module_path> <module>.wsgi --access-logfile '-' --error-logfile '-'
    ```    

    这些日志将显示在[应用服务日志流](#access-diagnostic-logs)中。

    有关详细信息，请参阅 [Gunicorn 日志记录](https://docs.gunicorn.org/en/stable/settings.html#logging) (docs.gunicorn.org)。
    
- **自定义 Flask 主模块**：默认情况下，应用服务假定 Flask 应用的主模块是 application.py 或 app.py 。 如果主模块使用其他名称，则必须自定义启动命令。 例如，如果 Flask 应用的主模块是 hello.py，而该文件中的 Flask 应用对象名为 `myapp`，则命令如下所示：

    ```bash
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```
    
    如果主模块位于子文件夹（例如 `website`）中，请使用 `--chdir` 参数指定该文件夹：
    
    ```bash
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir website hello:myapp
    ```
    
- **使用非 Gunicorn 服务器**：若要使用其他 web 服务器（如 [aiohttp](https://aiohttp.readthedocs.io/en/stable/web_quickstart.html)），请使用适当的命令作为启动命令或在启动命令文件中使用：

    ```bash
    python3.7 -m aiohttp.web -H localhost -P 8080 package.module:init_func
    ```

## <a name="access-app-settings-as-environment-variables"></a>作为环境变量访问应用设置

应用设置是专门为应用存储在云中的值，如[配置应用设置](configure-common.md#configure-app-settings)中所述。 这些设置可以作为环境变量提供给应用代码，并使用标准的 [os.environ](https://docs.python.org/3/library/os.html#os.environ) 模式进行访问。

例如，如果已创建名为 `DATABASE_SERVER` 的应用设置，则以下代码将检索该设置的值：

```python
db_server = os.environ['DATABASE_SERVER']
```
    
## <a name="detect-https-session"></a>检测 HTTPS 会话

在应用服务中，[SSL 终止](https://wikipedia.org/wiki/TLS_termination_proxy) (wikipedia.org) 在网络负载均衡器上发生，因此，所有 HTTPS 请求将以未加密的 HTTP 请求形式访问你的应用。 如果应用逻辑需要检查用户请求是否已加密，可以检查 `X-Forwarded-Proto` 标头。

```python
if 'X-Forwarded-Proto' in request.headers and request.headers['X-Forwarded-Proto'] == 'https':
# Do something when HTTPS is used
```

使用常用 Web 框架可以访问采用标准应用模式的 `X-Forwarded-*` 信息。 在 [CodeIgniter](https://codeigniter.com/) 中，[is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) 默认检查 `X_FORWARDED_PROTO` 的值。

## <a name="access-diagnostic-logs"></a>访问诊断日志

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

若要通过 Azure 门户访问日志，请在应用的左侧菜单中选择“监视” > “日志流” 。

## <a name="access-deployment-logs"></a>访问部署日志

当你部署代码时，应用服务会执行前面的[自定义生成自动化](#customize-build-automation)部分所述的生成过程。 由于生成过程在自己的容器中运行，因此，生成日志与应用的诊断日志分开存储。

通过以下步骤访问部署日志：

1. 在 Web 应用的 Azure 门户上，选择左侧菜单中的“部署” > “部署中心(预览版)”。
1. 在“日志”选项卡上，选择最新提交的“提交 ID”。
1. 在出现的“日志详细信息”页上，选择“正在运行 oryx 生成...”旁边显示的“显示日志...”链接。

生成问题（如 requirements.txt 中不正确的依赖项）以及生成前或生成后脚本中的错误都会显示在这些日志中。 如果 requirements 文件没有准确命名为 requirements.txt 或者没有出现在项目的根文件夹中，也会出现错误。

## <a name="open-ssh-session-in-browser"></a>在浏览器中打开 SSH 会话

[!INCLUDE [Open SSH session in browser](../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

成功连接到 SSH 会话后，应该会在窗口底部显示“已建立 SSH 连接”消息。 如果显示诸如“SSH_CONNECTION_CLOSED”之类的错误或容器重启消息，则表明可能有错误阻止应用容器启动。 有关调查可能存在的问题的步骤，请参阅[故障排除](#troubleshooting)。

## <a name="troubleshooting"></a>疑难解答

一般来说，故障排除的第一步是使用应用服务诊断：

1. 在 Web 应用的 Azure 门户上，从左侧菜单中选择“诊断并解决问题”。
1. 选择“可用性和性能”。
1. 查看“应用程序日志”、“容器崩溃”和“容器问题”选项中的信息，其中会出现最常见的问题。

接下来，查看[部署日志](#access-deployment-logs)和[应用日志](#access-diagnostic-logs)中是否有任何错误消息。 这些日志通常会标识可能阻止应用部署或应用启动的特定问题。 例如，如果 requirements.txt 文件具有错误的文件名或不在项目根文件夹中，则生成可能失败。

以下部分针对特定问题提供了更多指导。

- [应用未显示 - 显示默认应用](#app-doesnt-appear)
- [应用未显示 -“服务不可用”消息](#service-unavailable)
- [找不到 setup.py 或 requirements.txt](#could-not-find-setuppy-or-requirementstxt)
- [在 SSH 会话中键入密码时，密码不显示](#other-issues)
- [SSH 会话中的命令似乎已被截断](#other-issues)
- [静态资产未在 Django 应用中显示](#other-issues)
- [致命错误: 需要建立 SSL 连接](#other-issues)

#### <a name="app-doesnt-appear"></a>应用未显示

- **部署自己的应用代码后看到默认应用。** 之所以显示[默认应用](#default-behavior)，是因为并未将应用代码部署到应用服务，或者应用服务未找到应用代码，因此运行了默认应用。

    - 请重启应用服务，等待 15 到 20 秒，然后再次检查应用。
    
    - 请确保使用适用于 Linux 的应用服务，而不要使用基于 Windows 的实例。 在 Azure CLI 中运行 `az webapp show --resource-group <resource-group-name> --name <app-name> --query kind` 命令，对 `<resource-group-name>` 和 `<app-name>` 进行相应的替换。 应该会看到作为输出的 `app,linux`，否则请重新创建应用服务并选择 Linux。
    
    - 使用 [SSH](#open-ssh-session-in-browser) 直接连接到应用服务容器，并验证文件是否在 site/wwwroot 下。 如果不在，请执行以下步骤：
      1. 创建一个名为 `SCM_DO_BUILD_DURING_DEPLOYMENT` 且值为 1 的应用设置，重新部署代码，等待几分钟，然后再次尝试访问应用。 有关创建应用设置的详细信息，请参阅[在 Azure 门户中配置应用服务应用](configure-common.md)。
      1. 查看部署过程，[检查部署日志](#access-deployment-logs)，更正所有错误，然后重新部署应用。
    
    - 如果这些文件存在，则表示应用服务无法识别特定的启动文件。 检查是否按应用服务的预期方式为 [Django](#django-app) 或 [Flask](#flask-app) 构建了应用，或使用[自定义启动命令](#customize-startup-command)。

- <a name="service-unavailable"></a>**浏览器中显示“服务不可用”消息。** 浏览器在等待应用服务的响应时超时，这表示应用服务已启动 Gunicorn 服务器，但应用本身未启动。 这种情况可能表示 Gunicorn 参数不正确，或者应用代码有错误。

    - 刷新浏览器，尤其是在应用服务计划中使用最低定价层的情况下。 例如，使用免费层时，应用可能需要较长时间才能启动，并在刷新浏览器后才会做出响应。

    - 检查是否按应用服务的预期方式为 [Django](#django-app) 或 [Flask](#flask-app) 构建了应用，或使用[自定义启动命令](#customize-startup-command)。

    - 检查[应用日志流](#access-diagnostic-logs)是否有任何错误消息。 日志将显示应用代码中的任何错误。

#### <a name="could-not-find-setuppy-or-requirementstxt"></a>找不到 setup.py 或 requirements.txt

- 日志流显示“找不到 setup.py 或 requirements.txt；未运行 pip install。”：Oryx 生成过程找不到 requirements.txt 文件。

    - 通过 [SSH](#open-ssh-session-in-browser) 连接到 Web 应用的容器，并验证 requirements.txt 的命名是否正确，以及是否就在 site/wwwroot 下。 如果该文件不存在，请使该文件存在于存储库中，并包含在部署中。 如果它存在于单独的文件夹中，请将其移到根文件夹下。

#### <a name="other-issues"></a>其他问题

- **在 SSH 会话中键入密码时，密码不显示**：出于安全考虑，SSH 会话会在你键入密码时隐藏密码。 但是，这些字符会被记录下来，因此，请照常键入密码，完成后按 Enter。

- **SSH 会话中的命令似乎已被截断**：编辑器可能未对命令自动换行，但它们仍应正常运行。

- **静态资产未在 Django 应用中显示**：确保已启用 [WhiteNoise 模块](http://whitenoise.evans.io/en/stable/django.html)

- **你看到消息“致命错误: 需要建立 SSL 连接”** ：检查任何用于从应用内部访问资源（如数据库）的用户名和密码。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [教程：使用 PostgreSQL 的 Python 应用](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [教程：从专用容器存储库进行部署](tutorial-custom-container.md?pivots=container-linux)

> [!div class="nextstepaction"]
> [应用服务 Linux 常见问题解答](faq-app-service-linux.md)
