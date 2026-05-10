# 🛡️ Análisis de Smart Contracts | Ciberseguridad Web3 Defensiva

## 👤 Sobre mi rol
Soy **Analista de Smart Contracts** enfocado en la **Ciberseguridad Web3 Defensiva**. Mi labor principal consiste en examinar la lógica de los contratos inteligentes para detectar fallos, vulnerabilidades o errores de diseño antes de que el código sea desplegado. Mi objetivo es verificar la integridad del sistema y asegurar la protección de activos mediante reportes técnicos y recomendaciones de mejora.

> **⚠️ NOTA:** Este repositorio es un laboratorio educativo con código intencionalmente vulnerable para demostrar el proceso de análisis y mitigación en Web3.

---

## 🔎 Metodología de Análisis Defensivo

Como analista, realizo una lectura manual línea por línea para identificar vectores de ataque. En este caso, analicé el flujo de fondos del contrato `VulnerableBank.sol`.

### 1. Hallazgo: Reentrancy (Reentrada)
Detecté que el contrato entrega el control al exterior antes de actualizar el estado interno, lo que permite el vaciado de fondos.

```solidity
// PUNTO DE FALLO DETECTADO
(bool éxito, ) = msg.sender.call{value: monto}(""); 
saldos[msg.sender] = 0; 
```

### 2. Evaluación de Riesgo
Impacto: **Crítico**. Un error de lógica de este tipo permite que un atacante drene el balance total del protocolo debido a la falta de atomicidad en la transacción.

### 3. Mitigación (Defensa)
Propuse la corrección aplicando el patrón **Checks-Effects-Interactions**, asegurando que el saldo se actualice **antes** de la transferencia.

---

## 📄 Archivos del Análisis

Para este estudio de ciberseguridad web3, he preparado los siguientes archivos de forma independiente:

*   **[`VulnerableBank.sol`](./VulnerableBank.sol):** Código original con el fallo de lógica de negocio.
*   **[`SecureBank.sol`](./SecureBank.sol):** Código corregido con la implementación defensiva.
*   **[`Poc_Ataque.sol`](./Poc_Ataque.sol):** Prueba de concepto que demuestra cómo se explotaría el fallo.
*   **[`Reporte_Tecnico.pdf`](./Reporte_Tecnico.pdf):** Documento formal con el informe completo de la auditoría.

---

## 🛠️ Herramientas Utilizadas
*   **Visual Studio Code:** Análisis manual y revisión de lógica.
*   **Solidity Visual Developer:** Visualización de flujos y seguridad de funciones.
*   **Análisis Defensivo:** Simulación mental y técnica de estados de la EVM.

