# Crea tu token ERC20 con Vyper utilizando Apeworx

## Intro

En Web3 siempre tomamos en cuenta el tema de pluralidad, por ello aquí les muestro una opción para generar un token ERC20 utilizando Vyper, un lenguaje "Pythonico" para crear Smart Contracts, utilizando el framework de [Apeworx](https://www.apeworx.io/).

Lo desplegaremos en la testnet de goerli utilizando Infura para interactuar con la blockchain.

## Instalando Ape

Se  recomienda el uso de un entorno virtual usando python, para ello usaremos ``venv``:

    $ mkdir apeworx
    $ cd apeworx
    $ python3 -m venv venv

Ingresamos al entorno virtual, en mi caso utilizando linux:

    $ source pip venv/bin/activate

Nota: Debes tener una versión de Python superior a ``3.7.2``. También, tienes que tener instalado venv de lo contrario usa:

    $ sudo apt install python3.10-venv python3-pip

Luego seguimos la guía de Apeworx, e instalamos el último release de Apeworx via pip:

    $ pip install -U pip
    $ pip install eth-ape

En mi caso, también tuve que instalar la librería de vyper:

    $ pip install vyper

Instalamos el plugin de los templates que incluye la plantilla ERC20 en Vyper que vamos a utilizar:

    $ ape plugins install template

Clonamos el template de un ``ERC20``, e interactuamos con la consola indicando, principalmente que no sea un ERC4626 (por simplicidad en este caso), el nombre del token y su símbolo, el resto lo dejamos por default:
 
    $ ape template gh:ApeAcademy/ERC20

        ERC4626 [y]: n
        project_name [TokenProject]: Demo
        token_name [TokenTest]: MiPropioToken
        token_symbol [TKN]: MPT
        token_decimals [18]: 
        premint [y]: 
        premint_amount [1000]: 
        minter_role [y]: 
        mintable [y]: 
        burnable [y]: 
        permitable [y]: 

Ingresamos dentro de la carpeta generada:

    $ cd Demo

En la carpeta contracts, pueden ver que se ha creado el archivo "Token.vy", allí pueden revisar el detalle de la plantilla generada en Vyper.

Para revisar los plugins que tenemos instalados utilizamos el comando:

    $ ape plugins list
    Installed Plugins:
      template    0.5.0

Instalamos el plugin de Vyper:

    $ ape plugins install vyper
    INFO: Installing vyper...
    SUCCESS: Plugin 'vyper==0.5.2' has been installed.

Compilamos nuestro contrato:

    $ ape compile
    INFO: Compiling 'Token.vy'.

Importamos nuestra cuenta, en este caso nos referimos a la cuenta de prueba de Metamask (por ejemplo) desde donde vamos a desplegar el token:

    $ ape accounts import my_account

        Enter Private Key: 
        Create Passphrase to encrypt account: 
        Repeat for confirmation: 
        SUCCESS: A new account '0x....' has been added with the id 'my_account'

Algo que me parece valioso y muy importante es que a diferencia de Hardhat no se usa un archivo .env, por ejemplo, para mantener las claves en secreto sino que el mismo framework te ofrece tenerlos encriptados de esta manera.

Verificamos:

    $ ape accounts list

Vemos las redes disponibles para el despliegue:

    $ ape networks list
        ethereum  (default)
        ├── mainnet
        │   └── geth  (default)
        ├── goerli
        │   └── geth  (default)
        └── local  (default)
            ├── test  (default)
            └── geth

Para interactuar con Infura, instalamos el plugin correspondiente:

    $ ape plugins install ape-infura
    INFO: Installing infura...
    SUCCESS: Plugin 'infura==0.5.3' has been installed.

Seteamos nuestro PROJECT_ID:

    $ export WEB3_INFURA_PROJECT_ID=<>

Y luego deployamos el contrato:

    $ ape run deploy --network ethereum:goerli:infura

        DynamicFeeTransaction:
        chainId: 5
        from: 0x...
        gas: 712287
        nonce: 6
        value: 0
        data: 0x34610b...0080fd
        type: 0x02
        maxFeePerGas: 20146
        maxPriorityFeePerGas: 20133
        accessList: []

        Sign:  [y/N]: y
        Enter passphrase to unlock 'my_account' []: 
        Leave 'my_account' unlocked? [y/N]: y
        INFO: Submitted 0x...
        Confirmations (2/2): 100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 2/2 [00:28<00:00, 14.12s/it]
        INFO: Confirmed 0x... (total fees paid = 14349733902)
        SUCCESS: Contract 'Token' deployed to: 0x...

Y listo!!! Pueden ver su transacción en [Goerli Etherscan](https://goerli.etherscan.io/) Tenemos un token ERC20 desplegado en Testnet.

Luego de ello podemos realizar las variaciones que queramos también hay otras plantillas como las de un ERC721 por si quieren probar también.

Otra cosa bastante importante es que no se requiere mucho hardware, esta prueba por ejemplo la hice desde una Raspberry Pi 4 - 8G, y corrió sin problemas.