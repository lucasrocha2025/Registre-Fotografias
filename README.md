<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Gerador de Contrato Fotográfico</title>
  <style>
    body { display: flex; flex-wrap: wrap; font-family: Arial, sans-serif; gap: 20px; }
    .formulario { width: 300px; }
    .contrato { 
      border: 1px solid #000; 
      padding: 20px; 
      background: url('BACKGROUND.jpeg') no-repeat center center; 
      background-size: cover; 
      width: 600px; 
      min-height: 800px; 
      overflow: auto; 
      color: #000;
      white-space: pre-wrap;
    }
    input { width: 100%; margin-top: 5px; margin-bottom: 10px; padding: 5px; }
    button { padding: 10px; margin-top: 10px; cursor: pointer; }
  </style>
</head>
<body>

<div class="formulario">
  <h2>Preencha os dados:</h2>

  <label>Nome do Cliente:</label>
  <input type="text" id="nomeCliente">

  <label>Tipo de Evento:</label>
  <input type="text" id="tipoEvento">

  <label>Data do Evento:</label>
  <input type="date" id="dataEvento">

  <label>Duração (horas):</label>
  <input type="number" id="duracao">

  <label>Valor Total:</label>
  <input type="text" id="valor">

  <label>Data de Pagamento:</label>
  <input type="date" id="dataPagamento">

  <label>Pagamento Parcelado?</label>
  <input type="text" id="parcelado" placeholder="Sim ou Não">

  <label>Número de Parcelas:</label>
  <input type="number" id="numParcelas">

  <button id="gerarBtn">Gerar PDF</button>
</div>

<div class="contrato">
  <div id="textoContrato"></div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const campos = {
      nomeCliente: document.getElementById('nomeCliente'),
      tipoEvento: document.getElementById('tipoEvento'),
      dataEvento: document.getElementById('dataEvento'),
      duracao: document.getElementById('duracao'),
      valor: document.getElementById('valor'),
      dataPagamento: document.getElementById('dataPagamento'),
      parcelado: document.getElementById('parcelado'),
      numParcelas: document.getElementById('numParcelas')
    };

    const textoContrato = document.getElementById('textoContrato');
    const gerarBtn = document.getElementById('gerarBtn');
    const contrato = document.querySelector('.contrato');

    function atualizarContrato() {
      const nomeCliente = campos.nomeCliente.value || '[Nome do Cliente]';
      const primeiroNome = nomeCliente.split(' ')[0] || nomeCliente;

      textoContrato.innerHTML = `
<b>CONTRATANTE:</b> ${nomeCliente}<br>
<b>CONTRATADO:</b> Registre Fotografias<br><br>

As partes acima identificadas celebram o presente contrato de prestação de serviços fotográficos, conforme as cláusulas e condições a seguir:<br><br>

<b>RESUMO DO SERVIÇO</b><br>
- Tipo de evento: ${campos.tipoEvento.value || '[Tipo de Evento]'}<br>
- Data do evento: ${campos.dataEvento.value || '[Data do Evento]'}<br>
- Duração: ${campos.duracao.value || '[Duração]'} horas<br>
- Valor total: ${campos.valor.value || '[Valor Total]'}<br>
- Pagamento: ${campos.dataPagamento.value || '[Data de Pagamento]'}<br>
- Parcelado: ${campos.parcelado.value || '[Sim ou Não]'}<br>
- Número de Parcelas: ${campos.numParcelas.value || '[N° de Parcelas]'}<br><br>

<b>1. OBJETIVO DO CONTRATO</b><br>
Este contrato tem como objetivo a prestação de serviços de fotografia e produção de vídeo resumo por parte do CONTRATADO para o evento do CONTRATANTE, conforme briefing acordado entre as partes.<br><br>

<b>2. SERVIÇOS INCLUÍDOS</b><br>
- Cobertura fotográfica e vídeo resumo do evento;<br>
- Edição de imagens com padrão;<br>
- Entrega digital do material finalizado.<br><br>

<b>3. SERVIÇOS NÃO INCLUÍDOS</b><br>
O CONTRATADO não realiza serviços de design gráfico, animações ou divulgação do material produzido, salvo acordo adicional.<br><br>

<b>4. BRIEFING E COMUNICAÇÃO</b><br>
As especificações detalhadas do evento serão definidas em briefing conjunto. Toda comunicação será feita por WhatsApp, e-mail ou redes sociais autorizadas, em horários comerciais.<br><br>

<b>5. HORÁRIOS E EXCEDENTES</b><br>
- A duração do serviço é de ${campos.duracao.value || '[Duração]'} horas.<br>
- Horas extras serão cobradas à parte, a R$ 50,00 por hora adicional.<br>
- Solicitações feitas com menos de 2 dias de antecedência estarão sujeitas a taxa extra de 30%.<br><br>

<b>6. ENTREGA E PRAZOS</b><br>
- O material será entregue até 7 dias úteis após o evento, por link digital.<br>
- A seleção das fotos pelo CONTRATANTE deve ocorrer em até 7 dias após o envio. O atraso pode comprometer o cronograma de entrega.<br><br>

<b>7. REFAÇÕES E AJUSTES</b><br>
- 1 (uma) refação está inclusa sem custo.<br>
- Refações adicionais: R$ 50,00 cada.<br><br>

<b>8. DIREITOS E ARQUIVOS</b><br>
- O CONTRATADO manterá backup por 15 dias após a entrega.<br>
- Somente os arquivos editados serão entregues.<br>
- Os arquivos brutos não serão disponibilizados, salvo negociação.<br>
- O CONTRATADO poderá usar as imagens para portfólio, salvo objeção expressa do CONTRATANTE.<br><br>

<b>9. CANCELAMENTO E MULTAS</b><br>
- Cancelamentos com até 48h de antecedência: retenção de até 30% do valor.<br>
- Cancelamento após serviço iniciado: não há reembolso.<br>
- Cancelamento pelo CONTRATADO: devolução integral ou reagendamento.<br><br>

<b>10. INADIMPLEMENTO</b><br>
- Atraso no pagamento implica multa de 10%, juros de 1% ao mês e correção monetária.<br>
- Em caso de cobrança judicial, aplica-se 20% de honorários advocatícios.<br><br>

<b>11. VALOR E PAGAMENTO</b><br>
Valor total: ${campos.valor.value || '[Valor Total]'}<br>
Pagamento: ${campos.dataPagamento.value || '[Data de Pagamento]'}<br>
Parcelado: ${campos.parcelado.value || '[Sim ou Não]'}<br>
Número de Parcelas: ${campos.numParcelas.value || '[N° de Parcelas]'}<br><br>

<b>12. DISPOSIÇÕES FINAIS</b><br>
As partes elegem o foro da comarca de Várzea Grande/MT para dirimir quaisquer dúvidas. Firmado em 2 vias de igual teor.<br><br>

Local e data: ________, ___ de ______ de 2025.<br><br>

__________&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__________<br>
Assinatura do CONTRATADO&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Assinatura do CONTRATANTE<br>
Registre Fotografias&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;${primeiroNome}
      `;
    }

    for (let campo in campos) {
      campos[campo].addEventListener('input', atualizarContrato);
    }

    gerarBtn.addEventListener('click', function() {
      html2pdf().from(document.querySelector('.contrato')).save(${campos.nomeCliente.value}_contrato.pdf);
    });

    atualizarContrato();
  });
</script>

</body>
</html>
