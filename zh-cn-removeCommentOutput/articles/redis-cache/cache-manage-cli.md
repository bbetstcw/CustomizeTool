<properties 
	pageTitle="如何使用 Azure 命令行界面 (Azure CLI) 创建和管理 Azure Redis 缓存" 
	description="了解如何在任何平台上安装 Azure CLI、如何使用它连接到你的 Azure 帐户，以及如何从 Azure CLI 创建和管理 Redis 缓存。" 
	services="redis-cache" 
	documentationCenter="" 
	authors="steved0x" 
	manager="dwrede" 
	editor=""/>

<tags 
	ms.service="cache" 
	ms.date="09/16/2015" 
	wacn.date=""/>

# 如何使用 Azure 命令行界面 (Azure CLI) 创建和管理 Azure Redis 缓存

Azure CLI 是从任何平台管理 Azure 基础结构的好办法。本文演示了如何使用 Azure CLI 创建和管理 Azure Redis 缓存实例。

## 先决条件

若要使用 Azure CLI 创建和管理 Azure Redis 缓存实例，必须完成以下步骤。

-	你必须具有 Azure 帐户。如果你没有帐户，只需花费几分钟就能创建一个[免费试用帐户](/pricing/1rmb-trial/)。
-	[安装 Azure CLI](/documentation/articles/xplat-cli#install)。
-	将 Azure CLI 安装与个人 Azure 帐户或者工作或学校 Azure 帐户关联，然后使用 `azure login` 命令从 Azure CLI 登录。若要了解差别并进行选择，请参阅[如何连接到 Azure 订阅](/documentation/articles/xplat-cli#configure)。
-	在运行以下任何命令之前，通过运行以下命令将 Azure CLI 切换到资源管理器模式下。`azure config mode arm` 有关详细信息，请参阅[设置 Azure 资源管理器模式](/documentation/articles/xplat-cli-azure-resource-manager#setting-the-azure-resource-manager-mode)。

## Redis 缓存属性

在创建和更新 Redis 缓存实例时使用以下属性。

| 属性 | Switch | 说明 |
|------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| name | -n, --name | Redis 缓存的名称。 |
| 资源组 | -g, --resource-group | 资源的名称。 |
| location | -l, --location | 要创建缓存的位置。 |
| size | -z, --size | Redis 缓存的大小。有效值：[C0, C1, C2, C3, C4, C5, C6] |
| sku | -x, --sku | Redis SKU。应为以下值之一：[Basic, Standard] |
| MaxMemoryPolicy | -m, --max-memory-policy | Redis 缓存的 MaxMemoryPolicy 属性。有效值：[AllKeysLRU, AllKeysRandom, NoEviction, VolatileLRU, VolatileRandom, VolatileTTL] |
| EnableNonSslPort | -e, --enable-non-ssl-port | Redis 缓存的 EnableNonSslPort 属性。如果要为你的缓存启用非 SSL 端口，请添加此标志 |
| 订阅 | -s, --subscription | 订阅标识符。 |
| key type | -t, --key-type | 要续订的密钥类型。有效值：[Primary, Secondary] |

## 查看所有 Redis 缓存命令

若要查看所有 Redis 缓存命令及其参数，请使用 `azure rediscache -h` 命令。

	C:\>azure rediscache -h
	help:    Commands to manage your Azure Redis Cache(s)
	help:
	help:    Create a Redis Cache
	help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
	help:
	help:    Delete an existing Redis Cache
	help:      rediscache delete [--name <name> --resource-group <resource-group> ]
	help:
	help:    List all Redis Caches within your Subscription or Resource Group
	help:      rediscache list [options]
	help:
	help:    Show properties of an existing Redis Cache
	help:      rediscache show [--name <name> --resource-group <resource-group>]
	help:
	help:    Change settings of an existing Redis Cache
	help:      rediscache set [--name <name> --resource-group <resource-group> --max-memory-policy <max-memory-policy>]
	help:
	help:    Renew the authentication key for an existing Redis Cache
	help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
	help:
	help:    Lists Primary and Secondary key of an existing Redis Cache
	help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
	help:
	help:    Options:
	help:      -h, --help  output usage information
	help:
	help:    Current Mode: arm (Azure Resource Management)

## 创建 Redis 缓存

若要创建 Redis 缓存，请使用以下命令：

	azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

有关此命令的详细信息，请运行 `azure rediscache create -h` 命令。

	C:\>azure rediscache create -h
	help:    Create a Redis Cache
	help:
	help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
	help:
	help:    Options:
	help:      -h, --help                                   output usage information
	help:      -v, --verbose                                use verbose output
	help:      -vv                                          more verbose with debug output
	help:      --json                                       use json output
	help:      -n, --name <name>                            Name of the Redis Cache.
	help:      -g, --resource-group <resource-group>        Name of the Resource Group
	help:      -l, --location <location>                    Location to create cache.
	help:      -z, --size <size>                            Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6]
	help:      -x, --sku <sku>                              Redis SKU. Should be one of : [Basic, Standard]
	help:      -m, --max-memory-policy <max-memory-policy>  MaxMemoryPolicy property of the Redis Cache. Valid values: [AllKeysLRU, AllKeysRandom, NoEviction, VolatileLRU, VolatileRandom, VolatileTTL]
	help:      -e, --enable-non-ssl-port                    EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
	help:      -s, --subscription <id>                      the subscription identifier
	help:
	help:    Current Mode: arm (Azure Resource Management)

## 删除现有 Redis 缓存

若要删除 Redis 缓存，请使用以下命令：

	azure rediscache delete [--name <name> --resource-group <resource-group> ]

有关此命令的详细信息，请运行 `azure rediscache delete -h` 命令。

	C:\>azure rediscache delete -h
	help:    Delete an existing Redis Cache
	help:
	help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
	help:
	help:    Options:
	help:      -h, --help                             output usage information
	help:      -v, --verbose                          use verbose output
	help:      -vv                                    more verbose with debug output
	help:      --json                                 use json output
	help:      -n, --name <name>                      Name of the Redis Cache.
	help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
	help:      -s, --subscription <subscription>      the subscription identifier
	help:
	help:    Current Mode: arm (Azure Resource Management)

## 列出你的订阅或资源组中的所有 Redis 缓存

若要列出订阅或资源组中的所有 Redis 缓存，请使用以下命令：

	azure rediscache list [options]

有关此命令的详细信息，请运行 `azure rediscache list -h` 命令。

	C:\>azure rediscache list -h
	help:    List all Redis Caches within your Subscription or Resource Group
	help:
	help:    Usage: rediscache list [options]
	help:
	help:    Options:
	help:      -h, --help                             output usage information
	help:      -v, --verbose                          use verbose output
	help:      -vv                                    more verbose with debug output
	help:      --json                                 use json output
	help:      -g, --resource-group <resource-group>  Name of the Resource Group
	help:      -s, --subscription <subscription>      the subscription identifier
	help:
	help:    Current Mode: arm (Azure Resource Management)

## 显示现有 Redis 缓存的属性

若要显示现有 Redis 缓存的属性，请使用以下命令：

	azure rediscache show [--name <name> --resource-group <resource-group>]

有关此命令的详细信息，请运行 `azure rediscache show -h` 命令。

	C:\>azure rediscache show -h
	help:    Show properties of an existing Redis Cache
	help:
	help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
	help:
	help:    Options:
	help:      -h, --help                             output usage information
	help:      -v, --verbose                          use verbose output
	help:      -vv                                    more verbose with debug output
	help:      --json                                 use json output
	help:      -n, --name <name>                      Name of the Redis Cache.
	help:      -g, --resource-group <resource-group>  Name of the Resource Group
	help:      -s, --subscription <subscription>      the subscription identifier
	help:
	help:    Current Mode: arm (Azure Resource Management)

## 更改现有 Redis 缓存的设置

若要更改现有 Redis 缓存的设置，请使用以下命令：

	azure rediscache set [--name <name> --resource-group <resource-group> --max-memory-policy <max-memory-policy>]

有关此命令的详细信息，请运行 `azure rediscache set -h` 命令。

	C:\>azure rediscache set -h
	help:    Change settings of an existing Redis Cache
	help:
	help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --max-memory-policy <max-memory-policy>]
	help:
	help:    Options:
	help:      -h, --help                                   output usage information
	help:      -v, --verbose                                use verbose output
	help:      -vv                                          more verbose with debug output
	help:      --json                                       use json output
	help:      -n, --name <name>                            Name of the Redis Cache.
	help:      -g, --resource-group <resource-group>        Name of the Resource Group
	help:      -m, --max-memory-policy <max-memory-policy>  Max Memory Policy of the Redis Cache. Valid values: [AllKeysLRU, AllKeysRandom, NoEviction, VolatileLRU, VolatileRandom, VolatileTTL]
	help:      -s, --subscription <subscription>            the subscription identifier
	help:
	help:    Current Mode: arm (Azure Resource Management)

## 为现有 Redis 缓存续订身份验证密钥

若要为现有 Redis 缓存续订身份验证密钥，请使用以下命令：

	azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

为 `key-type` 指定 `Primary` 或 `Secondary`。

有关此命令的详细信息，请运行 `azure rediscache renew-key -h` 命令。

	C:\>azure rediscache renew-key -h
	help:    Renew the authentication key for an existing Redis Cache
	help:
	help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
	help:
	help:    Options:
	help:      -h, --help                             output usage information
	help:      -v, --verbose                          use verbose output
	help:      -vv                                    more verbose with debug output
	help:      --json                                 use json output
	help:      -n, --name <name>                      Name of the Redis Cache.
	help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
	help:      -t, --key-type <key-type>              type of key to renew
	help:      -s, --subscription <subscription>      the subscription identifier
	help:
	help:    Current Mode: arm (Azure Resource Management)

## 列出现有 Redis 缓存的主密钥和辅助密钥

若要列出现有 Redis 缓存的主密钥和辅助密钥，请使用以下命令：

	azure rediscache list-keys [--name <name> --resource-group <resource-group>]

有关此命令的详细信息，请运行 `azure rediscache list-keys -h` 命令。

	C:\>azure rediscache list-keys -h
	help:    Lists Primary and Secondary key of an existing Redis Cache
	help:
	help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
	help:
	help:    Options:
	help:      -h, --help                             output usage information
	help:      -v, --verbose                          use verbose output
	help:      -vv                                    more verbose with debug output
	help:      --json                                 use json output
	help:      -n, --name <name>                      Name of the Redis Cache.
	help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
	help:      -s, --subscription <subscription>      the subscription identifier
	help:
	help:    Current Mode: arm (Azure Resource Management)

<!---HONumber=74-->