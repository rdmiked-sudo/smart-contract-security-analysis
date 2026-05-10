# 🛡️ Análisis de Smart Contracts | Ciberseguridad Web3 Defensiva

## 👤 Sobre mi rol
Soy **Analista de Smart Contracts** enfocado en la **Ciberseguridad Web3 Defensiva**. Mi labor principal consiste en examinar la lógica de los contratos inteligentes para detectar fallos, vulnerabilidades o errores de diseño antes de que el código sea desplegado. A diferencia de un perfil ofensivo, mi objetivo es verificar la integridad del sistema y asegurar que los activos de los usuarios estén protegidos mediante reportes técnicos y recomendaciones de mejora.

> **⚠️ NOTA:** Este repositorio es un laboratorio educativo. Contiene código intencionalmente vulnerable para demostrar el proceso de análisis y mitigación de riesgos en entornos Web3.

---

## 🔎 Metodología de Análisis Defensivo

Como analista, sigo un proceso riguroso para auditar el código y reportar hallazgos de seguridad:

### 1. Análisis Estático y Revisión de Lógica
Utilizo **VS Code** para realizar una lectura manual línea por línea. En este caso de estudio, analicé el flujo de fondos del contrato `VulnerableBank.sol`, identificando una debilidad crítica en la gestión de estados.

**Vulnerabilidad identificada: Reentrancy (Reentrada)**
```solidity
function retirarTodo() public {
    uint256 monto = saldos[msg.sender];
    require(monto > 0, "No tienes fondos");

    // PUNTO DE FALLO: Se entrega el control al exterior antes de actualizar el balance
    (bool éxito, ) = msg.sender.call{value: monto}(""); 
    
    saldos[msg.sender] = 0; 
}
```

### 2. Evaluación de Riesgo en Ciberseguridad Web3
Al analizar el hallazgo, determino que el impacto es **Crítico**. Un atacante podría interceptar la ejecución y vaciar los fondos del contrato. Mi función como analista es prever estos escenarios de fallo en la lógica de negocio y reportarlos de inmediato.

### 3. Propuesta de Mitigación (Defensa)
Para asegurar el contrato, propongo la implementación del patrón **Checks-Effects-Interactions**, garantizando que el estado interno se actualice antes de cualquier interacción con una dirección externa.

---

## 📁 Estructura del Repositorio

Para facilitar la navegación por este análisis de seguridad, el repositorio se organiza de la siguiente manera:

*   **[`/contracts`](./contracts):** Archivos fuente de Solidity.
    *   `VulnerableBank.sol`: El contrato original que contiene el fallo de lógica detectado.
    *   `SecureBank.sol`: La versión corregida con las medidas defensivas aplicadas.
*   **[`/report`](./report):** Documentación formal del análisis.
    *   `Reporte_Analisis_H01.pdf`: Informe técnico detallado con severidad, impacto y solución.
*   **[`/poc`](./poc):** (Proof of Concept) Código de prueba utilizado para validar cómo la lógica vulnerable podría ser comprometida.

---

## 🛠️ Herramientas de mi día a día
Para este análisis de seguridad web3 defensiva, he utilizado:
*   **Visual Studio Code:** Para el análisis manual de código fuente.
*   **Solidity Visual Developer:** Extensión para visualizar la visibilidad y flujo de las funciones.
*   **Análisis de Lógica:** Simulación de estados para verificar la robustez del contrato.
