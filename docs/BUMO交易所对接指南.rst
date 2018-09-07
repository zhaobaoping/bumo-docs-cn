 BUMO交易所对接指南
==================

概述
----

本文档用于交易所对接BUMO节点、安装部署以及使用BUMO SDK。

安装BUMO节点
------------

支持 Ubuntu、Centos 等大多数操作系统编译，推荐使用版本Ubuntu
14.04或Centos 7。本示例将以 Ubuntu 14.04 作为节点的安装示例。

安装依赖
~~~~~~~~

在编译BUMO的源代码之前需要安装系统所需的依赖。安装依赖需要完成以下步骤：

1. 输入以下命令安装 ``automake``。

::

   sudo apt-get install automake

2. 输入以下命令安装 ``autoconf``。

::

   sudo apt-get install autoconf

3. 输入以下命令安装 ``libtool``。

::

   sudo apt-get install libtool

4. 输入以下命令安装 ``g++``。

::

   sudo apt-get install g++

5. 输入以下命令安装 ``libssl-dev``。

::

   sudo apt-get install libssl-dev

6. 输入以下命令安装 ``cmake``。

::

   sudo apt-get install cmake

7. 输入以下命令安装 ``libbz2-dev``。

::

   sudo apt-get install libbz2-dev

8. 输入以下命令安装 ``python``。

::

   sudo apt-get install python

9. 输入以下命令安装 ``unzip``。

::

   sudo apt-get install unzip

编译源代码
~~~~~~~~~~

在成功安装依赖后才能编译BUMO的源代码。如果没有安装 ``git``，可以通过 ``sudo apt-get install git`` 命令来安装 ``git``。编译BUMO的源代码需要完成以下步骤：

1. 在根目录下输入以下命令下载BUMO的源代码文件。

::

   git clone https://github.com/bumoproject/bumo.git

|image0|

.. note:: 在BUMO的源代码下载过程中将自动创建bumo/目录，源代码文件将存放到该目录下。

2. 输入以下命令进入到源代码的文件目录。

::

   cd /bumo/build/

3. 输入以下命令下载依赖并初始化开发环境。

::

   ./install-build-deps-linux.sh

4. 输入以下命令回到bumo/目录下。

::

   cd ../

5. 输入以下命令完成BUMO源代码的编译。出现下图所示信息则表示编译成功。

::

   make

|image1|

.. note:: 编译完成后生成的可执行文件 **bumo** 和 **bumod** 存放在/bumo/bin目录下。

.. _安装bumo节点-1:

安装BUMO节点
~~~~~~~~~~~~

在编译完成后才能安装BUMO节点。安装BUMO节点需要完成以下步骤：

1. 输入以下命令进入到安装目录。

::

   cd /bumo/

2. 输入以下命令完成安装。出现下图所示信息则表示安装成功。

::

   make install

|image2|

.. note:: |
  - 默认情况下服务安装在/usr/local/buchain/目录下。
  - 安装完成后无需其他配置即可通过 ``service bumo start`` 命令来启动bumo服务。
  - 安装完BUMO节点后在buchain/目录下有如下目录结构：

+---------+------------------------------------------+
| 目录    | 说明                                     |
+=========+==========================================+
| bin     | 存放可执行文件（编译后的bumo可执行程序） |
+---------+------------------------------------------+
| jslib   | 存放第三方js库                           |
+---------+------------------------------------------+
| config  | 配置文件目录包含：bumo.json              |
+---------+------------------------------------------+
| data    | 数据库目录，存放账本数据                 |
+---------+------------------------------------------+
| scripts | 启停脚本目录                             |
+---------+------------------------------------------+
| log     | 运行日志存储目录                         |
+---------+------------------------------------------+

更改运行环境
~~~~~~~~~~~~

在更改BUMO的运行环境之前需要关闭BUMO服务 。您可按照以下步骤进行修改。

1. 输入以下命令进入到配置文件目录。

::

   cd /usr/local/buchain/config/

.. note:: | 在该目录下提供了以下运行环境的配置文件。
  - bumo-mainnet.json（该文件是主网环境的配置文件，应用在生成环境中） 
  - bumo-testnet.json（该文件是测试网环境的配置文件）
  - bumo-single.json（该文件是单节点调试环境的配置文件）

2. 把当前运行环境的配置文件（bumo.json）更改为其他名称，例如：

::

   mv bumo.json bumoprevious.json

3. 把要运行的环境配置文件更改为bumo.json，例如：

::

   mv bumo-mainnet.json bumo.json

.. note:: 本示例中把主网环境设置成了运行环境。更改运行环境后需要清空数据库才能重启bumo服务。

运维服务
--------

在运维服务中对BUMO服务的启动、关闭、状态查询、系统详情查询、清空数据库进行了详细说明。

**启动BUMO服务**

输入以下命令启动bumo服务。

::

   service bumo start

**关闭BUMO服务**

输入以下命令关闭bumo服务。

::

   service bumo stop

**查询BUMO服务状态**

输入以下命令查询bumo服务。

::

   service bumo status

**查询系统详细状态**

输入以下命令查询系统详细状态：

::

   curl 127.0.0.1:19333/getModulesStatus

得到如下结果：

::

   {
    "glue_manager":{
        "cache_topic_size":0,
        "ledger_upgrade":{
            "current_states":null,
            "local_state":null
        },
        "system":{
            "current_time":"2017-07-20 10:32:22", //当前系统时间
            "process_uptime":"2017-07-20 09:35:06", //bumo启动时间
            "uptime":"2017-05-14 23:51:04"
        },
        "time":"0 ms",
        "transaction_size":0
    },
    "keyvalue_db":Object{...},
    "ledger_db":Object{...},
    "ledger_manager":{
        "account_count":2316,  //账户数
        "hash_type":"sha256",
        "ledger_sequence":12187,
        "time":"0 ms",
        "tx_count":1185   //交易数
    },
    "peer_manager":Object{...},
    "web server":Object{...},


**清空数据库**

在清空数据之前需要停止BUMO服务。清空数据库需要完成以下步骤：

1. 输入以下命令进入bumo的服务目录。

::

   /usr/local/buchain/bin

2. 输入以下命令清空数据库。

::

   ./bumo --dropdb

.. note:: 数据库成功清空后能看到如下所示的信息。

|image3|

JAVA SDK 用法说明
-----------------

JAVA
SDK的使用包括了 生成用户充值地址_ 、检测账户地址的合法性_ 以及 资产交易_。

生成用户充值地址
~~~~~~~~~~~~~~~~

交易所需要给每一个用户生成一个充值地址，交易所可通过 Bumo-sdk-java
中提供的Keypair.generator()创建用户的充值地址，具体示例如下所示：

::

   /**

        * 生成账户私钥、公钥以及地址
        */
       @Test
       public void createAccount() {
           Keypair keypair = Keypair.generator();
           System.out.println(JSON.toJSONString(keypair, true));
       }

返回值如下所示：

|image5|

检测账户地址的合法性
~~~~~~~~~~~~~~~~~~~~

通过如下所示代码检测账户地址的合法性。

::

   /**

        * 检验账户地址是否合法
        */
       @Test
       public void checkAccountAddress() {
           String address = "buQemmMwmRQY1JkcU7w3nhruoX5N3j6C29uo";
           AccountCheckValidRequest accountCheckValidRequest = new AccountCheckValidRequest();
           accountCheckValidRequest.setAddress(address);
           AccountCheckValidResponse accountCheckValidResponse = sdk.getAccountService().checkValid(accountCheckValidRequest);
           if (0 == accountCheckValidResponse.getErrorCode()) {
               System.out.println(accountCheckValidResponse.getResult().isValid());
           } else {
               System.out.println(JSON.toJSONString(accountCheckValidResponse, true));
           }
       }

.. note:: |
  - 如果返回值为 ``true`` 则表示账户地址合法 
  - 如果返回值为 ``false`` 则表示账户地址非法

资产交易
~~~~~~~~

在BUMO
网络里，每10秒产生一个区块，每个交易只需要一次确认即可得到交易终态。在本章节将介绍 `探测用户充值`_ 、`用户提现或转账`_ 以及 `查询交易`_ 。

探测用户充值
^^^^^^^^^^^^

交易所需要开发监听区块生成，然后解析区块里的交易记录，从而确认用户充值行为。具体步骤如下:

1. 确保节点区块状态正常。

2. 解析区块里包含的交易（解析方法见解析区块交易）。

3. 记录解析后的结果。

**查看区块状态**

通过如下所示代码查看区块状态。

::

   /**

        * 检测连接的节点是否区块同步正常
        */
       @Test
       public void checkBlockStatus() {
           BlockCheckStatusResponse response = sdk.getBlockService().checkStatus();
           System.out.println(response.getResult().getSynchronous());
       }

.. note:: |
  - 如果返回值为 ``true`` 则表示区块正常 
  - 如果返回值为 ``false`` 则表示区块异常


**解析区块交易**

交易所可根据区块高度查询该区块里的交易信息，然后分析每条交易信息。

请求示例：

::

   /**

        * 探测用户充值操作
        * 
        * 通过解析区块中的交易来探测用户的充值动作
        */
       @Test
       public void getTransactionOfBolck() {
           Long blockNumber = 617247L;// 第617247个区块
           BlockGetTransactionsRequest request = new BlockGetTransactionsRequest();
           request.setBlockNumber(blockNumber);
           BlockGetTransactionsResponse response = sdk.getBlockService().getTransactions(request);
           if (0 == response.getErrorCode()) {
               System.out.println(JSON.toJSONString(response, true));
           } else {
               System.out.println("Failure\n" + JSON.toJSONString(response, true));
           }
           // 探测某个账户是否充值BU
           // 解析transactions[n].transaction.operations[n].pay_coin.dest_address 

           // 注意：
           // Operations是数组，有可能有多笔转账操作
       }

响应报文如下：

::

  {
	"total_count": 1,
	"transactions": [{
		"close_time": 1524467568753121,
		"error_code": 0,
		"error_desc": "",
		"hash": "89402813097402d1983c178c5ec271c6890db40c3beb9f06db71c8d52dab6c86",
		"ledger_seq": 33063,
		"signatures": [{
			"public_key": "b001dbf0942450f5601e39ac1f7223e332fe0324f1f91ec16c286258caba46dd29f6ef9bf93b",
			"sign_data": "668984fc7ded2dd30d87a1577f78eeb34d2198de3485be14ea66d9ca18f21aa21b2e0461ad8fedefc1abcb4221d346b404e8f9f9bd9c93a7df99baffeb616e0a"
		}],
		"transaction": {
			"fee_limit": 1000000,
			"gas_price": 1000,
			"metadata": "333133323333",
			"nonce": 25,
			"operations": [{
				"pay_coin": {
					"amount": 3000,
					"dest_address": "buQctxUa367fjw9jegzMVvdux5eCdEhX18ME"
				},
				"type": 7
			}],
			"source_address": "buQhP7pzmjoRsNG7AkhfNxiWd7HuYsYnLa4x"
		}
	}]
  }

  响应报文解释：

  total_count    交易总数（一般情况下都是1）
  transactions   查询区块中交易对象，数组大小是该区块的交易总数
  actual_fee     交易费用，单位是MO
  close_time     交易时间
  error_code     交易状态 0 是成功 非0 为失败
  error_desc     交易状态信息
  hash           交易哈希
  ledger_seq     区块高度
  signatures     签名信息
  public_key     签名者公钥
  sign_data      签名者签名数据
  transaction    签名对象
  fee_limit      费用最小值，单位 MO
  gas_price      Gas，单位 MO
  metadata       交易附加信息
  nonce          交易原账号交易数
  operations     操作对象(支持多个)
  pay_coin       操作类型：内置token
  amount         转移BU数量，单位 MO
  dest_address   接收方地址
  type           操作类型：7 为内置token转移
  source_address 转出方地址


.. note:: |
  - 关于Bumo-sdk-java 如何使用，请访问以下链接：

    https://github.com/bumoproject/bumo-sdk-java/tree/release2.0.0

  - 关于交易所对接示例，请访问以下链接：

    https://github.com/bumoproject/bumo-sdk-java/blob/release2.0.0/examples/src/main/java/io/bumo/sdk/example/ExchangeDemo.java

用户提现或转账
^^^^^^^^^^^^^^

用户提现操作可参考bumo-sdk-java 提供的转账示例，如下所示：

::

   /**
        * 发送一笔BU交易
        *
        * @throws Exception
        */
       @Test
       public void sendBu() throws Exception {
           // 初始化变量
           // 发送方私钥
           String senderPrivateKey = "privbyQCRp7DLqKtRFCqKQJr81TurTqG6UKXMMtGAmPG3abcM9XHjWvq";
           // 接收方账户地址
           String destAddress = "buQswSaKDACkrFsnP1wcVsLAUzXQsemauE";
           // 发送BU数量
           Long amount = ToBaseUnit.BU2MO("0.01");
           // 固定写 1000L，单位是MO
           Long gasPrice = 1000L;
           // 设置最大费用 0.01BU
           Long feeLimit = ToBaseUnit.BU2MO("0.01");
           // 参考getAccountNonce()获取账户Nonce+ 1
           Long nonce = 1L;

           // 记录 txhash，以便后续再次确认交易真实结果
           // 推荐5个区块后通过txhash再次调用`根据交易Hash获取交易信息`（参考提示：getTxByHash()）来确认交易终态结果
           String txhash = sendBu(senderPrivateKey, destAddress, amount, nonce, gasPrice, feeLimit);

       }

.. note:: |
  - 记录提现操作的hash值，以便后续查看该笔提现操作的终态结果
  - ``gasPrice`` 目前（2018-04-23）最低值是1000MO
  - ``feeLimit`` 建议填写1000000MO，即0.01BU

查询交易
^^^^^^^^

用户提现操作的终态结果可通过当时发起提现操作时返回的hash值进行查询。

调用示例如下所示：
::

 public static void queryTransactionByHash(BcQueryService queryService) {
   String txHash = "";
   TransactionHistory tx = queryService.getTransactionHistoryByHash(txHash);
   System.out.println(tx);
 }

.. note:: |
  - ``tx.totalCount`` 数量大于等于1时说明交易历史存在
  - ``tx.transactions.errorCode`` 等于0表示交易成功，非0表示交易失败，具体原因查看 ``errorDesc``
  - 用户提现操作，交易所请关注 ``pay_coin`` 操作
  - 完整用户提现响应示例：

::

  {
	"total_count": 1,
	"transactions": [{
		"close_time": 1524467568753121,
		"error_code": 0,
		"error_desc": "",
		"hash": "89402813097402d1983c178c5ec271c6890db40c3beb9f06db71c8d52dab6c86",
		"ledger_seq": 33063,
		"signatures": [{
			"public_key": "b001dbf0942450f5601e39ac1f7223e332fe0324f1f91ec16c286258caba46dd29f6ef9bf93b",
			"sign_data": "668984fc7ded2dd30d87a1577f78eeb34d2198de3485be14ea66d9ca18f21aa21b2e0461ad8fedefc1abcb4221d346b404e8f9f9bd9c93a7df99baffeb616e0a"
		}],
		"transaction": {
			"fee_limit": 1000000,
			"gas_price": 1000,
			"metadata": "333133323333",
			"nonce": 25,
			"operations": [{
				"pay_coin": {
					"amount": 3000,
					"dest_address": "buQctxUa367fjw9jegzMVvdux5eCdEhX18ME"
				},
				"type": 7
			}],
			"source_address": "buQhP7pzmjoRsNG7AkhfNxiWd7HuYsYnLa4x"
		}
	}]
  }
  total_count    交易总数（一般情况下都是1）
  transactions   查询区块中交易对象，数组大小是该区块的交易总数
  actual_fee     交易费用，单位是MO
  close_time     交易时间
  error_code     交易状态 0 是成功 非0 为失败
  error_desc     交易状态信息
  hash           交易哈希
  ledger_seq     区块高度
  signatures     签名信息
  public_key     签名者公钥
  sign_data      签名者签名数据
  transaction    签名对象
  fee_limit      费用最小值，单位 MO
  gas_price      Gas，单位 MO
  metadata       交易附加信息
  nonce          交易原账号交易数
  operations     操作对象(支持多个)
  pay_coin       操作类型：内置token
  amount         转移BU数量，单位 MO
  dest_address   接收方地址
  type           操作类型：7 为内置token转移
  source_address 转出方地址


BU-Explorer
-----------

BUMO提供了区块链数据浏览工具，可供用户查询区块数据。

您访问以下链接查询区块链数据：

- 测试网：http://explorer.bumotest.io
- 主网：http://explorer.bumo.io

BUMO钱包
--------

BUMO提供了Windows和Mac版全节点钱包，可供用户管理用户私钥、查看BU余额转账以及离线签名交易等功能。

您可以通过以链接下载BUMO钱包：

https://github.com/bumoproject/bumo-wallet/releases

常见问题
--------

**BUChain命令行的节点启动**

问：使用BUChain命令行时是否需要启动该节点？

答：不用。

**gas_price和fee_limit的值是否固定**

问：``gas_price`` 是固定1000MO，``fee_limit`` 是1000000MO 吗？

答：不是固定。但目前(2018-04-23) ``gas_price`` 是1000MO，``gas_price``
越大越优先打包。``fee_limit`` 是交易时交易发起方最多给区块链的交易费用，在正常合法的交易情况下区块链收取的真实费用小于调用方填写的 ``fee_limit`` 。( ``gas_price``
可通过\ http://seed1.bumo.io:16002/getLedger?with_fee=true\ 查询的结果 ``result.fees.gas_price`` 字段得到）。

**账户余额转出** 

问：账户的余额能否全部转出？

答：不能。为了防止DDOS
攻击，防止创建大量垃圾账户，BUMO激活的账户必须保留一定数量的BU，目前是0.1BU（可通过\ http://seed1.bumo.io:16002/getLedger?with_fee=true
查询的结果 ``result.fees.base_reserve`` 字段得到）。

.. |image0| image:: ../docs/image/download_bumo_back2.png
.. |image1| image:: ../docs/image/compile_finished.png
.. |image2| image:: ../docs/image/compile_installed.png
.. |image3| image:: ../docs/image/clear_database.png
.. |image4| image:: ../docs/image/BU-Ex-API-JAVA-v1.0.jpg
.. |image5| image:: ../docs/image/2.jpg
.. |image6| image:: ../docs/image/3.jpg
.. |image7| image:: ../docs/image/4.jpg
.. |image8| image:: ../docs/image/5.jpg
.. |image9| image:: ../docs/image/1.png
.. |image10| image:: ../docs/image/2.png
.. |image11| image:: ../docs/image/3.png
.. |image12| image:: ../docs/image/6.jpg
.. |image13| image:: ../docs/image/7.jpg
.. |image14| image:: ../docs/image/4.png
.. |image15| image:: ../docs/image/5.png

