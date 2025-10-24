let movimientos = [];

function agregarMovimiento() {
  const descripcion = document.getElementById('descripcion').value.trim();
  const monto = parseFloat(document.getElementById('monto').value);
  const tipo = document.getElementById('tipo').value;

  if (!descripcion || isNaN(monto) || monto <= 0) {
    alert("Por favor ingrese una descripción y un monto válido.");
    return;
  }

  const movimiento = { descripcion, monto, tipo };
  movimientos.push(movimiento);

  actualizarTotales();
  mostrarMovimientos();
  
  document.getElementById('descripcion').value = "";
  document.getElementById('monto').value = "";
}

function actualizarTotales() {
  const ingresos = movimientos.filter(m => m.tipo === 'ingreso').reduce((acc, m) => acc + m.monto, 0);
  const gastos = movimientos.filter(m => m.tipo === 'gasto').reduce((acc, m) => acc + m.monto, 0);
  const balance = ingresos - gastos;

  document.getElementById('ingresosTotal').textContent = "S/ " + ingresos.toFixed(2);
  document.getElementById('gastosTotal').textContent = "S/ " + gastos.toFixed(2);
  document.getElementById('balanceTotal').textContent = "S/ " + balance.toFixed(2);
}

function mostrarMovimientos(filtro = 'todos') {
  const contenedor = document.getElementById('listaMovimientos');
  contenedor.innerHTML = "";

  let lista = movimientos;
  if (filtro === 'ingresos') lista = movimientos.filter(m => m.tipo === 'ingreso');
  if (filtro === 'gastos') lista = movimientos.filter(m => m.tipo === 'gasto');

  lista.forEach(mov => {
    const div = document.createElement('div');
    div.classList.add('movimiento');
    div.innerHTML = `
      <span>${mov.descripcion}</span>
      <span>${mov.tipo === 'ingreso' ? '+' : '-'} S/ ${mov.monto.toFixed(2)}</span>
    `;
    contenedor.appendChild(div);
  });
}

function showSection(tipo) {
  mostrarMovimientos(tipo);
}
