# ERC20-Token-
Un contrato inteligente ERC-20 para la creación de un token en la blockchain Ethereum.
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract SimuladorToken is ERC20 {
    address public owner;
    mapping(address => bool) public users;

    // Evento para emitir cuando un usuario interactúa con el contrato
    event UsuarioRegistrado(address usuario);
    event TokensAcunados(address usuario, uint256 cantidad);
    event TokensTransferidos(address desde, address a, uint256 cantidad);

    constructor(string memory name, string memory symbol) ERC20(name, symbol) {
        owner = msg.sender;
    }

    modifier soloOwner() {
        require(msg.sender == owner, "Solo el propietario puede realizar esta accion");
        _;
    }

    // Función para registrar un usuario y simular que interactúa con el contrato
    function registrarUsuario(address usuario) public soloOwner {
        users[usuario] = true;
        emit UsuarioRegistrado(usuario);
    }

    // Función para acuñar tokens y asignarlos al usuario
    function acuñarTokens(uint256 cantidad) public {
        require(users[msg.sender], "Debe estar registrado para acuñar tokens");
        _mint(msg.sender, cantidad * (10 ** decimals()));
        emit TokensAcunados(msg.sender, cantidad);
    }

    // Función para transferir tokens entre usuarios
    function transferirTokens(address to, uint256 cantidad) public {
        require(users[msg.sender], "Debe estar registrado para transferir tokens");
        _transfer(msg.sender, to, cantidad * (10 ** decimals()));
        emit TokensTransferidos(msg.sender, to, cantidad);
    }

    // Función para verificar el balance de un usuario
    function verificarBalance(address usuario) public view returns (uint256) {
        return balanceOf(usuario);
    }
}
