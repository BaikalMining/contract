# Смарт-контракт Baikal Mining

[![BaikalMining](https://baikal-mining.io/img/bam/logo_dark.png)](https://baikal-mining.io/img/bam/logo_dark.png)

Смарт-контракт содержит:

  - Переменные и константы
    - `address public owner` Адрес текущего владельца контракта
    - `mapping (address => uint256) public balances` Записи о балансах токенов
    - `mapping (address => mapping (address => uint256)) allowed`
    - `string public standard = 'Baikal Mining'`
    - `string public constant name = "Baikal Mining"` Полное название токена
    - `string public constant symbol = "BAM"` Сокращенное название токена
    - `uint public constant decimals = 18` Кол-во знаков после запятой
    - `uint public constant totalSupply = 34550000 * 1000000000000000000;` общее кол-во токенов в wei
    - `uint internal tokenPrice = 700000000000000` текущая стоимость 1 токена в wei
    - `bool public buyAllowed = true` определяет возможность покупки токенов, прямой отправкой эфира на контракт
    - `bool public transferBlocked = true` определяет возможность перевода токенов со счета на счет
  
  - События, генерирующиеся в сети блок-чейн при проведении транзакций определеного типа:
    - `event Buy(address indexed sender, uint eth, uint fbt)` событие покупки токенов
    - `event Withdraw(address indexed sender, address to, uint eth)` событие списания эфира с адреса контракта
    - `event Transfer(address indexed from, address indexed to, uint256 value)` событие отправки токенов
    - `event Approval(address indexed _owner, address indexed _spender, uint256 _value)` событие о разрешение отправки токенов

  - Модификаторы:
    - `modifier onlyOwner()` Модификатор: позволяет сделать вызов функции только владельцу контракта
    - `modifier onlyOwnerIfBlocked()` если владелец заблокировал отправку токенов, то только он может вызвать функцию с этим модификатором

  - Методы:
    - `function Bam() public` Конструктор контракта (вызывается один раз, при публикации контракта в сети)
    - `function() public payable` Функция, вызывающаяся при прямой отправке эфира на адрес контракта
    - `function transferOwnership(address newOwner) public onlyOwner` Позволяет владельцу контракта передать владение другому адресу
    - `function buyTokens(address _buyer) public payable` Функция, обеспечивающая покупку токенов в автоматическом режиме, за эфир
    - `function setTokenPrice(uint _newPrice) public` Установить стоимость одного токена в Wei
    - `function getTokenPrice() public view returns (uint price)` Получить стоимость одного токена в Wei
    - `function setBuyAllowed(bool _allowed) public` Включение/отключение возможности автоматической покупки токенов
    - `function setTransferBlocked(bool _blocked) public` Разблокировать/заблокировать возможность отправки токенов со счета на счет
    - `function withdrawEther(address _to) public` Отправить эфир с баланса контракта на адрес его владельца
    - `function safeMul(uint a, uint b) internal pure returns (uint)` Функция для безопасного выполенения операции умножения
    - `function safeSub(uint a, uint b) internal pure returns (uint)` Функция для безопасного выполенения операции вычитания
    - `function safeAdd(uint a, uint b) internal pure returns (uint)` Функция для безопасного выполенения операции сложения

  - Методы стандарта ERC 20:
    - `function transfer(address _to, uint256 _value) public onlyOwnerIfBlocked returns (bool success)` отправка токена на указанный адрес
    - `function transferFrom(address _from, address _to, uint256 _value) public onlyOwnerIfBlocked returns (bool success)` отправка токена с указанного адреса на другой адрес
    - `function approve(address _spender, uint256 _value) public onlyOwnerIfBlocked returns (bool success)` разрешение отправки токенов указанному адресу
    - `function allowance(address _owner, address _spender) public onlyOwnerIfBlocked constant returns (uint256 remaining)` метод для проверки кол-ва токенов, разрешенных к отправке указанному адресу, на определенном аккаунте
    
    > полное описание стандарта ERC 20 доступно по ссылке https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20.md)
