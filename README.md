<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    :root {
      --red: #e60014;
      --red-dark: #c90012;
      --ink: #222831;
      --muted: #687184;
      --border: #e1e5eb;
      --surface: #fff;
      --canvas: #f4f6f8;
      --green: #14804a;
      --orange: #b45309;
      --blue: #2563eb;
    }
    * { box-sizing: border-box; }
    body { margin: 0; font-family: Arial, Helvetica, sans-serif; color: var(--ink); background: var(--canvas); }
    button, input, select, textarea { font: inherit; }
    .app { min-height: 100vh; display: grid; grid-template-columns: 232px 1fr; }
    .sidebar { background: #1f242b; color: white; padding: 24px 16px; position: sticky; top: 0; height: 100vh; }
    .brand { display: flex; align-items: center; gap: 10px; font-size: 21px; font-weight: 700; margin-bottom: 28px; }
    .brand-mark { color: var(--red); font-size: 30px; line-height: 1; }
    .nav { display: grid; gap: 6px; }
    .nav button { border: 0; border-radius: 6px; color: #d9dee7; background: transparent; padding: 12px 14px; text-align: left; cursor: pointer; }
    .nav button.active, .nav button:hover { color: white; background: var(--red); }
    main { min-width: 0; padding: 28px 32px 50px; }
    .topbar { display: flex; align-items: center; justify-content: space-between; gap: 16px; margin-bottom: 24px; }
    h1 { margin: 0 0 4px; font-size: 28px; }
    .subtitle { color: var(--muted); margin: 0; }
    .user { display: flex; align-items: center; gap: 10px; }
    .avatar { width: 38px; height: 38px; border-radius: 50%; display: grid; place-items: center; background: #ffdce0; color: var(--red-dark); font-weight: bold; }
    .view { display: none; }
    .view.active { display: block; }
    .tabs { display: flex; gap: 8px; border-bottom: 1px solid var(--border); margin-bottom: 18px; overflow-x: auto; }
    .tab { border: 0; border-bottom: 3px solid transparent; background: transparent; padding: 12px 16px 10px; cursor: pointer; font-weight: 700; white-space: nowrap; }
    .tab.active { border-bottom-color: var(--red); color: var(--red-dark); }
    .tab small { margin-left: 6px; border-radius: 20px; padding: 2px 7px; background: #edf0f4; color: var(--muted); }
    .filters { display: flex; gap: 12px; align-items: end; padding: 16px; background: var(--surface); border: 1px solid var(--border); border-radius: 8px; margin-bottom: 16px; }
    .field { display: grid; gap: 6px; min-width: 170px; }
    .field.grow { flex: 1; }
    label { font-size: 13px; font-weight: 700; }
    input, select { height: 38px; border: 1px solid #ccd2dc; border-radius: 5px; padding: 0 10px; background: white; }
    .btn { border: 0; border-radius: 5px; min-height: 38px; padding: 0 16px; cursor: pointer; font-weight: 700; }
    .btn-primary { color: white; background: var(--red); }
    .btn-primary:hover { background: var(--red-dark); }
    .btn-secondary { color: var(--ink); border: 1px solid #ccd2dc; background: white; }
    .stats { display: grid; grid-template-columns: repeat(3, minmax(180px, 1fr)); gap: 14px; margin-bottom: 16px; }
    .stat { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; padding: 16px; }
    .stat span { color: var(--muted); font-size: 13px; }
    .stat strong { display: block; font-size: 24px; margin-top: 8px; }
    .panel { background: var(--surface); border: 1px solid var(--border); border-radius: 8px; overflow: hidden; }
    .panel-head { min-height: 58px; padding: 14px 16px; display: flex; align-items: center; justify-content: space-between; gap: 12px; border-bottom: 1px solid var(--border); }
    .panel-head h2 { font-size: 17px; margin: 0; }
    table { width: 100%; border-collapse: collapse; }
    th, td { padding: 13px 14px; border-bottom: 1px solid var(--border); text-align: left; font-size: 14px; }
    th { color: #596173; background: #f8f9fb; font-size: 12px; text-transform: uppercase; letter-spacing: .3px; }
    tbody tr:hover { background: #fafbfc; }
    .status { border-radius: 20px; padding: 5px 9px; font-size: 12px; font-weight: 700; white-space: nowrap; }
    .status.confirmar { background: #fff1c2; color: var(--orange); }
    .status.agendada { background: #dff3e7; color: var(--green); }
    .status.sem-agenda { background: #fde2e4; color: var(--red-dark); }
    .actions { display: flex; gap: 8px; }
    .icon-btn { border: 1px solid var(--border); border-radius: 5px; width: 34px; height: 34px; background: white; cursor: pointer; }
    .empty { text-align: center; color: var(--muted); padding: 38px !important; }
    .cards { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
    .card { background: white; border: 1px solid var(--border); border-radius: 8px; padding: 20px; }
    .card strong { font-size: 28px; display: block; margin-top: 6px; }
    .dropzone { border: 2px dashed #bbc4d0; border-radius: 8px; background: white; padding: 42px; text-align: center; cursor: pointer; }
    .dropzone.drag { border-color: var(--red); background: #fff5f6; }
    .helper { color: var(--muted); font-size: 13px; }
    .toast { position: fixed; right: 24px; bottom: 24px; padding: 14px 18px; border-radius: 6px; color: white; background: #273142; opacity: 0; transform: translateY(12px); transition: .2s; pointer-events: none; }
    .toast.show { opacity: 1; transform: translateY(0); }
    .modal-backdrop { position: fixed; inset: 0; z-index: 10; display: none; place-items: center; padding: 20px; background: rgba(0,0,0,.45); }
    .modal-backdrop.open { display: grid; }
    .modal { width: min(760px, 100%); max-height: 90vh; overflow: auto; border-radius: 8px; background: white; box-shadow: 0 18px 50px rgba(0,0,0,.25); }
    .modal-head { display: flex; justify-content: space-between; align-items: center; padding: 16px 20px; border-bottom: 1px solid var(--border); }
    .modal-head h2 { margin: 0; font-size: 19px; }
    .modal-body { display: grid; grid-template-columns: 1fr 1fr; gap: 18px; padding: 20px; }
    .modal-body .field { min-width: 0; }
    textarea { min-height: 88px; resize: vertical; border: 1px solid #ccd2dc; border-radius: 5px; padding: 10px; }
    .log-list { border-top: 1px solid var(--border); padding-top: 12px; max-height: 220px; overflow: auto; }
    .log-item { padding: 10px 0; border-bottom: 1px solid var(--border); font-size: 13px; }
    @media (max-width: 700px) { .modal-body { grid-template-columns: 1fr; } }
    .mobile-menu { display: none; }
    @media (max-width: 900px) {
      .app { grid-template-columns: 1fr; }
      .sidebar { height: auto; position: static; padding: 14px; }
      .brand { margin: 0; }
      .nav { display: none; grid-template-columns: repeat(4, 1fr); margin-top: 14px; }
      .nav.open { display: grid; }
      .nav button { text-align: center; }
      .mobile-menu { display: block; margin-left: auto; color: white; background: transparent; border: 1px solid #69717d; border-radius: 5px; padding: 8px 12px; }
      main { padding: 20px 16px; }
      .topbar { align-items: flex-start; }
      .filters { flex-wrap: wrap; }
      .field { flex: 1 1 180px; }
      .stats, .cards { grid-template-columns: 1fr; }
      .table-wrap { overflow-x: auto; }
      table { min-width: 850px; }
    }
  </style>
</head>
<body>
  <div class="app">
    <aside class="sidebar">
      <div class="brand"><button class="mobile-menu" type="button" onclick="toggleMenu()">Menu</button><span class="brand-mark">Claro—</span></div>
      <nav class="nav" id="nav">
        <button class="active" data-view="agenda">Agenda</button>
        <button data-view="importar">Importar arquivo</button>
        <button data-view="relatorio">Relatório</button>
        <button data-view="logs">Importar logs</button>
        <button data-view="configuracoes">Configurações</button>
      </nav>
    </aside>
    <main>
      <header class="topbar">
        <div><h1>Sistema de Agenda</h1><p class="subtitle" id="pageSubtitle">Gestão centralizada das tratativas</p></div>
        <div class="user"><div><strong>Operações</strong><br><span class="helper">Equipe de atendimento</span></div><div class="avatar">OP</div></div>
      </header>

      <section id="agenda" class="view active">
        <div class="tabs">
          <button class="tab active" data-tab="confirmacao">Confirmação <small id="countConfirmacao">0</small></button>
          <button class="tab" data-tab="com-agenda">Backlog com agenda <small id="countComAgenda">0</small></button>
          <button class="tab" data-tab="sem-agenda">Backlog sem agenda <small id="countSemAgenda">0</small></button>
        </div>
        <div class="filters">
          <div class="field grow"><label for="busca">Buscar contrato, cliente ou cidade</label><input id="busca" placeholder="Digite para pesquisar"></div>
          <div class="field"><label for="uf">UF</label><select id="uf"><option value="">Todas</option><option>SP</option><option>RJ</option><option>MG</option><option>BA</option><option>GO</option><option>PE</option></select></div>
          <div class="field"><label for="assistenteFiltro">Assistente</label><select id="assistenteFiltro" onchange="renderTable()"><option value="">Todos</option><option>Não distribuído</option><option>Marina</option><option>Carlos</option><option>Juliana</option></select></div>
          <div class="field"><label for="data">Data do agendamento</label><input id="data" type="date"></div>
          <button class="btn btn-primary" onclick="renderTable()">Filtrar</button>
          <button class="btn btn-secondary" onclick="limparFiltros()">Limpar</button>
        </div>
        <div class="stats">
          <div class="stat"><span>Total de contratos</span><strong id="statTotal">0</strong></div>
          <div class="stat"><span>Com agenda confirmada</span><strong id="statAgenda">0</strong></div>
          <div class="stat"><span>Aguardando confirmação</span><strong id="statConfirmacao">0</strong></div>
        </div>
        <div class="panel">
          <div class="panel-head"><h2 id="tableTitle">Tratativas para confirmação</h2><button class="btn btn-secondary" onclick="exportarCsv()">Exportar CSV</button></div>
          <div class="table-wrap"><table><thead><tr><th>Contrato</th><th>Cliente</th><th>UF</th><th>Cidade</th><th>Data</th><th>Assistente</th><th>Status</th><th>Ações</th></tr></thead><tbody id="tableBody"></tbody></table></div>
        </div>
      </section>

      <section id="importar" class="view">
        <div class="panel">
          <div class="panel-head"><h2>Importar contratos</h2></div>
          <div style="padding:20px">
            <label class="dropzone" id="dropzone"><strong>Selecionar arquivo de contratos</strong><br><span class="helper">Formatos aceitos: CSV ou XLSX</span><input id="fileContrato" type="file" accept=".csv,.xlsx" hidden></label>
            <p class="helper">Campos esperados: contrato, cliente, uf, cidade, data.</p>
            <p class="helper"><strong>Distribuição opcional:</strong> inclua a coluna <code>assistente</code> para atribuir cada contrato na importação.</p>
            <button class="btn btn-primary" style="margin-top:12px" onclick="importarArquivo()">Importar contratos</button>
          </div>
        </div>
      </section>

      <section id="relatorio" class="view">
        <div class="cards">
          <div class="card"><span class="helper">Contratos importados</span><strong id="reportTotal">0</strong></div>
          <div class="card"><span class="helper">Com agenda</span><strong id="reportAgenda">0</strong></div>
          <div class="card"><span class="helper">Sem agenda</span><strong id="reportSemAgenda">0</strong></div>
        </div>
        <div class="panel" style="margin-top:16px">
          <div class="panel-head"><h2>Resumo por UF</h2></div>
          <div class="table-wrap"><table><thead><tr><th>UF</th><th>Total</th><th>Confirmados</th><th>Sem agenda</th></tr></thead><tbody id="reportUf"></tbody></table></div>
        </div>
      </section>

      <section id="logs" class="view">
        <div class="panel">
          <div class="panel-head"><h2>Importar logs</h2></div>
          <div style="padding:20px">
            <label class="dropzone" id="dropzoneLogs"><strong>Selecionar arquivo de logs</strong><br><span class="helper">Formato aceito: CSV</span><input id="fileLogs" type="file" accept=".csv" hidden></label>
            <p class="helper">Os registros encontrados serão associados aos contratos existentes pelo número do contrato.</p>
            <button class="btn btn-primary" style="margin-top:12px" onclick="importarLogs()">Importar logs</button>
          </div>
        </div>
      </section>

      <section id="configuracoes" class="view">
        <div class="cards">
          <div class="card">
            <h2>Assistentes</h2>
            <p class="helper">Informe um nome por linha.</p>
            <textarea id="listaAssistentes" style="width:100%"></textarea>
          </div>
          <div class="card">
            <h2>Resoluções das tratativas</h2>
            <p class="helper">Informe uma opção por linha. Para alterar o destino, use Resultado|com-agenda, Resultado|sem-agenda ou Resultado|confirmacao.</p>
            <textarea id="listaResultados" style="width:100%;min-height:180px"></textarea>
          </div>
        </div>
        <button class="btn btn-primary" style="margin-top:16px" onclick="salvarConfiguracoes()">Salvar configurações</button>
        <button class="btn btn-secondary" style="margin-top:16px" onclick="restaurarConfiguracoes()">Restaurar padrões</button>
      </section>
    </main>
  </div>
  <div class="modal-backdrop" id="contratoModal" onclick="fecharModalFora(event)">
    <section class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitulo">
      <div class="modal-head"><h2 id="modalTitulo">Tratativa do contrato</h2><button class="icon-btn" onclick="fecharModal()" aria-label="Fechar">×</button></div>
      <div class="modal-body">
        <div>
          <div class="field"><label>Resultado da tratativa</label><select id="resultadoNota"><option value="">Selecione</option></select></div>
          <div class="field" style="margin-top:14px"><label>Observações</label><textarea id="observacaoNota" placeholder="Descreva o contato ou retorno recebido"></textarea></div>
        </div>
        <div>
          <div class="field"><label>Assistente responsável</label><select id="assistenteNota"><option value="">Não distribuído</option></select></div>
          <div class="field" style="margin-top:14px"><label>Histórico de notas</label><div class="log-list" id="historicoNotas"></div></div>
        </div>
        <div style="grid-column:1/-1;display:flex;justify-content:flex-end;gap:8px">
          <button class="btn btn-secondary" onclick="fecharModal()">Cancelar</button>
          <button class="btn btn-primary" onclick="salvarNota()">Salvar tratativa</button>
        </div>
      </div>
    </section>
  </div>
  <div class="toast" id="toast"></div>
  <script>
    const seed = [
      { contrato: '1002458', cliente: 'Comércio Central Ltda', uf: 'SP', cidade: 'São Paulo', data: '2026-11-05', status: 'confirmacao', assistente: 'Marina', notas: [] },
      { contrato: '1002461', cliente: 'Residencial Aurora', uf: 'RJ', cidade: 'Niterói', data: '2026-11-06', status: 'confirmacao', assistente: 'Carlos', notas: [] },
      { contrato: '1002488', cliente: 'Hospital Vida Plena', uf: 'MG', cidade: 'Belo Horizonte', data: '2026-11-07', status: 'com-agenda', assistente: 'Juliana', notas: [] },
      { contrato: '1002510', cliente: 'Condomínio Atlântico', uf: 'BA', cidade: 'Salvador', data: '', status: 'sem-agenda' },
      { contrato: '1002534', cliente: 'Metalúrgica Norte', uf: 'GO', cidade: 'Goiânia', data: '', status: 'sem-agenda' }
    ];
    let contratos = JSON.parse(localStorage.getItem('agendaContratos') || 'null') || seed;
    let abaAtual = 'confirmacao';
    let contratoModalAtual = -1;
    const configuracoesPadrao = {
      assistentes: ['Marina', 'Carlos', 'Juliana'],
      resultados: [
        { nome: 'Confirmado', destino: 'com-agenda' },
        { nome: 'Reagendado', destino: 'confirmacao' },
        { nome: 'Aberto cancelamento', destino: 'sem-agenda' },
        { nome: 'Endereço divergente', destino: 'sem-agenda' },
        { nome: 'Sem contato', destino: 'sem-agenda' },
        { nome: 'Sem WhatsApp', destino: 'sem-agenda' }
      ]
    };
    let configuracoes = JSON.parse(localStorage.getItem('agendaConfiguracoes') || 'null') || configuracoesPadrao;
    contratos.forEach(c => { c.assistente = c.assistente || ''; c.notas = c.notas || []; });

    document.querySelectorAll('.nav button').forEach(btn => btn.addEventListener('click', () => abrirView(btn.dataset.view)));
    document.querySelectorAll('.tab').forEach(tab => tab.addEventListener('click', () => {
      document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
      tab.classList.add('active');
      abaAtual = tab.dataset.tab;
      renderTable();
    }));
    document.getElementById('busca').addEventListener('input', renderTable);
    function salvar() { localStorage.setItem('agendaContratos', JSON.stringify(contratos)); }
    function abrirView(id) {
      document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
      document.getElementById(id).classList.add('active');
      document.querySelectorAll('.nav button').forEach(b => b.classList.toggle('active', b.dataset.view === id));
      document.getElementById('pageSubtitle').textContent = { agenda: 'Gestão centralizada das tratativas', importar: 'Carga de contratos a serem tratados', relatorio: 'Indicadores operacionais da agenda', logs: 'Atualização de status por arquivo de logs', configuracoes: 'Cadastro de assistentes e resultados' }[id];
      document.getElementById('nav').classList.remove('open');
      if (id === 'relatorio') renderRelatorio();
      if (id === 'configuracoes') carregarConfiguracoes();
    }
    function toggleMenu() { document.getElementById('nav').classList.toggle('open'); }
    function limparFiltros() {
      document.getElementById('busca').value = '';
      document.getElementById('uf').value = '';
      document.getElementById('assistenteFiltro').value = '';
      document.getElementById('data').value = '';
      atualizarListasConfiguradas();
      renderTable();
    }
    function formataData(v) {
      if (!v) return '-';
      const [ano, mes, dia] = v.split('-');
      return `${dia}/${mes}/${ano}`;
    }
    function statusLabel(status) {
      return status === 'confirmacao' ? ['confirmar', 'A confirmar'] : status === 'com-agenda' ? ['agendada', 'Com agenda'] : ['sem-agenda', 'Sem agenda'];
    }
    function renderTable() {
      const busca = document.getElementById('busca').value.toLowerCase();
      const uf = document.getElementById('uf').value;
      const data = document.getElementById('data').value;
      const assistente = document.getElementById('assistenteFiltro').value;
      const lista = contratos.filter(c =>
        c.status === abaAtual &&
        (!busca || [c.contrato, c.cliente, c.cidade].join(' ').toLowerCase().includes(busca)) &&
        (!uf || c.uf === uf) && (!data || c.data === data) &&
        (!assistente || (assistente === 'Não distribuído' ? !c.assistente : c.assistente === assistente)));
      document.getElementById('tableBody').innerHTML = lista.length ? lista.map((c, i) => {
        const realIndex = contratos.indexOf(c);
        const [classe, texto] = statusLabel(c.status);
        const notas = c.notas ? c.notas.length : 0;
        return `<tr><td><strong>${c.contrato}</strong></td><td>${c.cliente}</td><td>${c.uf}</td><td>${c.cidade}</td><td>${formataData(c.data)}</td><td>${c.assistente || '<span class="helper">Não distribuído</span>'}</td><td><span class="status ${classe}">${texto}</span></td><td><div class="actions"><button class="icon-btn" title="Confirmar" onclick="confirmar(${realIndex})">✓</button><button class="icon-btn" title="Sem agenda" onclick="semAgenda(${realIndex})">−</button><button class="icon-btn" title="Notas" onclick="abrirNota(${realIndex})">📝${notas ? `<small>${notas}</small>` : ''}</button></div></td></tr>`;
      }).join('') : '<tr><td colspan="8" class="empty">Nenhum contrato encontrado nesta lista.</td></tr>';
      const titulos = { confirmacao: 'Tratativas para confirmação', 'com-agenda': 'Backlog com agenda', 'sem-agenda': 'Backlog sem agenda' };
      document.getElementById('tableTitle').textContent = titulos[abaAtual];
      atualizarContadores();
    }
    function atualizarContadores() {
      const confirmacao = contratos.filter(c => c.status === 'confirmacao').length;
      const comAgenda = contratos.filter(c => c.status === 'com-agenda').length;
      const semAgenda = contratos.filter(c => c.status === 'sem-agenda').length;
      document.getElementById('countConfirmacao').textContent = confirmacao;
      document.getElementById('countComAgenda').textContent = comAgenda;
      document.getElementById('countSemAgenda').textContent = semAgenda;
      document.getElementById('statTotal').textContent = contratos.length;
      document.getElementById('statAgenda').textContent = comAgenda;
      document.getElementById('statConfirmacao').textContent = confirmacao;
    }
    function confirmar(index) {
      contratos[index].status = 'com-agenda';
      salvar(); renderTable(); toast('Contrato confirmado e movido para o backlog com agenda.');
    }
    function semAgenda(index) {
      contratos[index].status = 'sem-agenda';
      contratos[index].data = '';
      salvar(); renderTable(); toast('Contrato movido para o backlog sem agenda.');
    }
    function atualizarListasConfiguradas() {
      const assistentes = configuracoes.assistentes || [];
      const filtro = document.getElementById('assistenteFiltro');
      filtro.innerHTML = '<option value="">Todos</option><option>Não distribuído</option>' + assistentes.map(nome => `<option>${nome}</option>`).join('');
      const selectNota = document.getElementById('assistenteNota');
      selectNota.innerHTML = '<option value="">Não distribuído</option>' + assistentes.map(nome => `<option>${nome}</option>`).join('');
      document.getElementById('resultadoNota').innerHTML = '<option value="">Selecione</option>' +
        configuracoes.resultados.map(item => `<option value="${item.nome}">${item.nome}</option>`).join('');
    }
    function carregarConfiguracoes() {
      document.getElementById('listaAssistentes').value = configuracoes.assistentes.join('\n');
      document.getElementById('listaResultados').value = configuracoes.resultados.map(item => `${item.nome}|${item.destino}`).join('\n');
    }
    function salvarConfiguracoes() {
      const assistentes = document.getElementById('listaAssistentes').value.split('\n').map(v => v.trim()).filter(Boolean);
      const destinosValidos = ['confirmacao', 'com-agenda', 'sem-agenda'];
      const resultados = document.getElementById('listaResultados').value.split('\n').map(linha => {
        const partes = linha.split('|');
        return { nome: (partes[0] || '').trim(), destino: destinosValidos.includes((partes[1] || '').trim()) ? partes[1].trim() : 'confirmacao' };
      }).filter(item => item.nome);
      if (!assistentes.length || !resultados.length) { toast('Informe ao menos um assistente e uma resolução.'); return; }
      configuracoes = { assistentes, resultados };
      localStorage.setItem('agendaConfiguracoes', JSON.stringify(configuracoes));
      atualizarListasConfiguradas(); carregarConfiguracoes(); toast('Configurações atualizadas.');
    }
    function restaurarConfiguracoes() {
      configuracoes = JSON.parse(JSON.stringify(configuracoesPadrao));
      localStorage.removeItem('agendaConfiguracoes');
      atualizarListasConfiguradas(); carregarConfiguracoes(); toast('Configurações padrão restauradas.');
    }
    function abrirNota(index) {
      contratoModalAtual = index;
      const contrato = contratos[index];
      document.getElementById('modalTitulo').textContent = `Tratativa do contrato ${contrato.contrato}`;
      document.getElementById('assistenteNota').value = contrato.assistente || '';
      document.getElementById('resultadoNota').value = '';
      document.getElementById('observacaoNota').value = '';
      document.getElementById('historicoNotas').innerHTML = contrato.notas.length ? contrato.notas.map(n =>
        `<div class="log-item"><strong>${n.resultado}</strong><br>${n.observacao || '-'}<br><span class="helper">${n.data}</span></div>`
      ).join('') : '<span class="helper">Nenhuma nota lançada.</span>';
      document.getElementById('contratoModal').classList.add('open');
    }
    function fecharModal() { document.getElementById('contratoModal').classList.remove('open'); contratoModalAtual = -1; }
    function fecharModalFora(event) { if (event.target.id === 'contratoModal') fecharModal(); }
    function salvarNota() {
      if (contratoModalAtual < 0) return;
      const contrato = contratos[contratoModalAtual];
      contrato.assistente = document.getElementById('assistenteNota').value;
      const resultado = document.getElementById('resultadoNota').value;
      const observacao = document.getElementById('observacaoNota').value.trim();
      if (resultado || observacao) {
        contrato.notas.push({ resultado: resultado || 'Observação', observacao, data: new Date().toLocaleString('pt-BR') });
      }
      const resultadoConfigurado = configuracoes.resultados.find(item => item.nome === resultado);
      if (resultadoConfigurado) contrato.status = resultadoConfigurado.destino;
      salvar(); fecharModal(); renderTable(); toast('Tratativa salva no contrato.');
    }
    function toast(mensagem) {
      const el = document.getElementById('toast');
      el.textContent = mensagem; el.classList.add('show');
      setTimeout(() => el.classList.remove('show'), 2600);
    }
    function lerCsv(file, callback) {
      if (!file) { toast('Selecione um arquivo.'); return; }
      const leitor = new FileReader();
      leitor.onload = () => callback(leitor.result);
      leitor.readAsText(file);
    }
    function analisarCsv(texto) {
      const linhas = texto.trim().split(/\r?\n/).filter(Boolean);
      const cabecalho = linhas.shift().split(';')[0].includes(',') ? linhas.shift() : cabecalho;
      return linhas.map(linha => linha.split(/[,;]/).map(v => v.trim().replace(/^"|"$/g, '')));
    }
    function importarArquivo() {
      const file = document.getElementById('fileContrato').files[0];
      lerCsv(file, texto => {
        const linhas = texto.trim().split(/\r?\n/).filter(Boolean);
        const cabecalho = linhas.shift().split(/[;,]/).map(v => v.trim().toLowerCase().replace(/"/g, ''));
        let incluidos = 0;
        linhas.forEach(linha => {
          const valores = linha.split(/[;,]/).map(v => v.trim().replace(/^"|"$/g, ''));
          const item = {};
          cabecalho.forEach((col, i) => item[col] = valores[i] || '');
          if (item.contrato && !contratos.some(c => c.contrato === item.contrato)) {
            contratos.push({ contrato: item.contrato, cliente: item.cliente || '-', uf: (item.uf || '').toUpperCase(), cidade: item.cidade || '-', data: item.data || '', status: 'confirmacao', assistente: item.assistente || '', notas: [] });
            incluidos++;
          }
        });
        salvar(); renderTable(); toast(`${incluidos} contrato(s) importado(s).`);
      });
    }
    function importarLogs() {
      const file = document.getElementById('fileLogs').files[0];
      lerCsv(file, texto => {
        let atualizados = 0;
        texto.split(/\r?\n/).slice(1).forEach(linha => {
          const valores = linha.split(/[;,]/).map(v => v.replace(/^"|"$/g, '').trim());
          const contrato = valores[0];
          const registro = contratos.find(c => c.contrato === contrato);
          if (registro) {
            registro.status = 'com-agenda';
            if (valores[1]) registro.notas.push({ resultado: valores[1], observacao: valores[2] || '', data: new Date().toLocaleString('pt-BR') });
            atualizados++;
          }
        });
        salvar(); renderTable(); toast(`${atualizados} contrato(s) atualizado(s) pelos logs.`);
      });
    }
    function exportarCsv() {
      const linhas = [['Contrato', 'Cliente', 'UF', 'Cidade', 'Data', 'Assistente', 'Status']];
      contratos.filter(c => {
        const assistente = document.getElementById('assistenteFiltro').value;
        return c.status === abaAtual && (!assistente || (assistente === 'Não distribuído' ? !c.assistente : c.assistente === assistente));
      }).forEach(c => linhas.push([c.contrato, c.cliente, c.uf, c.cidade, formataData(c.data), c.assistente || '', statusLabel(c.status)[1]]));
      const csv = linhas.map(l => l.map(v => `"${v}"`).join(';')).join('\n');
      const url = URL.createObjectURL(new Blob([csv], { type: 'text/csv;charset=utf-8;' }));
      const a = document.createElement('a'); a.href = url; a.download = `agenda_${abaAtual}.csv`; a.click();
      URL.revokeObjectURL(url);
    }
    function renderRelatorio() {
      const porUf = {};
      contratos.forEach(c => { porUf[c.uf] ||= { total: 0, confirmados: 0, sem: 0 }; porUf[c.uf].total++; if (c.status === 'com-agenda') porUf[c.uf].confirmados++; if (c.status === 'sem-agenda') porUf[c.uf].sem++; });
      document.getElementById('reportTotal').textContent = contratos.length;
      document.getElementById('reportAgenda').textContent = contratos.filter(c => c.status === 'com-agenda').length;
      document.getElementById('reportSemAgenda').textContent = contratos.filter(c => c.status === 'sem-agenda').length;
      document.getElementById('reportUf').innerHTML = Object.entries(porUf).map(([uf, d]) => `<tr><td><strong>${uf}</strong></td><td>${d.total}</td><td>${d.confirmados}</td><td>${d.sem}</td></tr>`).join('');
    }
    ['dropzone', 'dropzoneLogs'].forEach(id => {
      const el = document.getElementById(id);
      el.addEventListener('dragover', e => { e.preventDefault(); el.classList.add('drag'); });
      el.addEventListener('dragleave', () => el.classList.remove('drag'));
      el.addEventListener('drop', e => { e.preventDefault(); el.classList.remove('drag'); const input = el.querySelector('input'); if (input) input.files = e.dataTransfer.files; });
    });
    renderTable();
  </script>
</body>
</html>
