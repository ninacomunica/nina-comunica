# Como conectar o Nina Comunica ao Google Planilhas

Este guia permite que a equipe da UBS atualize horários, serviços, campanhas e contato
diretamente em uma planilha do Google, sem mexer no código do site.

---

## Como vai funcionar

```
Equipe da UBS edita a planilha → salva → site atualiza automaticamente
```

---

## PARTE 1 — Criar a planilha

1. Acesse **sheets.google.com** com a conta Google da UBS.
2. Clique em **+** para criar uma nova planilha em branco.
3. Renomeie para: **Nina Comunica – Dados**

---

## PARTE 2 — Criar as abas com os dados

A planilha precisa ter **4 abas**, com os nomes e colunas exatos abaixo.

---

### Aba 1: `Horarios`

| Dia | Horario | Servico | Obs |
|-----|---------|---------|-----|
| Segunda-feira | 07:00 – 11:00 | Consulta médica | Agendamento |
| Segunda-feira | 13:00 – 16:00 | Vacinação | Livre demanda |
| Terça-feira | 07:00 – 11:00 | Consulta médica | Agendamento |
| Terça-feira | 08:00 – 10:00 | Grupo Hiperdia | Sala de reunião |
| Quarta-feira | 07:00 – 11:00 | Pré-natal | Agendamento |
| Quarta-feira | 15:00 – 17:00 | Atividade física (eMulti) | Área externa |
| Quinta-feira | 07:00 – 11:00 | Consulta médica | Agendamento |
| Quinta-feira | 13:00 – 16:00 | Curativo / Procedimentos | Livre demanda |
| Sexta-feira | 07:00 – 11:00 | Coleta de sangue | Em jejum |
| Sexta-feira | 07:00 – 10:00 | Dispensação de medicamentos | Livre demanda |

---

### Aba 2: `Servicos`

| Titulo | Descricao |
|--------|-----------|
| Consulta médica | Atendimento clínico geral, renovação de receitas e encaminhamentos. |
| Vacinação | Vacinas do calendário nacional para todas as idades, inclusive gripe. |
| Pré-natal | Acompanhamento da gestação com consultas e exames de rotina. |
| Puericultura | Acompanhamento do crescimento e desenvolvimento das crianças. |
| Grupo Hiperdia | Grupo de apoio para pacientes com hipertensão e diabetes. |
| Coleta de sangue | Coleta de exames laboratoriais às sextas-feiras, em jejum. |
| Curativo e procedimentos | Realização de curativos simples e procedimentos de baixa complexidade. |
| Dispensação de medicamentos | Retirada de medicamentos da farmácia básica do SUS. |
| Atividades físicas (eMulti) | Exercícios orientados pelo educador físico nas quartas-feiras. |
| Acolhimento | Escuta qualificada e direcionamento para o serviço mais adequado. |

---

### Aba 3: `Campanhas`

| Dia | Mes | Titulo | Descricao |
|-----|-----|--------|-----------|
| 12 | Nov | Novembro Azul – Saúde do Homem | Roda de conversa e orientações · 9h, sala de reunião |
| 19 | Nov | Dia Mundial da Diabetes | Aferição de glicemia gratuita · 8h – 11h, recepção |
| 26 | Nov | Grupo Hiperdia – Novembro | Encontro mensal · Terça-feira, 8h – 10h |
| 03 | Dez | Campanha de Vacinação Infantil | Atualização do cartão vacinal · Livre demanda |
| 10 | Dez | Atividade de Encerramento do Ano | Celebração com a comunidade · 9h, área externa |

---

### Aba 4: `Contato`

| Campo | Valor |
|-------|-------|
| telefone | (81) 9 9999-9999 |
| endereco | Rua das Flores, s/n · Bairro Nina Liberato · Caruaru – PE |
| horarioFuncionamento | Segunda a Sexta: 07h às 17h |
| mapsLink | https://maps.google.com/?q=UBS+Nina+Liberato+Caruaru |

> ⚠️ Na aba Contato, a coluna **Campo** deve ter exatamente esses nomes (sem acento, sem maiúscula diferente).

---

## PARTE 3 — Publicar a planilha como CSV

Para cada aba, você vai gerar um link público de leitura:

1. No menu superior, clique em **Arquivo → Compartilhar → Publicar na web**.
2. Na janela que abrir:
   - Em **Link**, selecione a aba (ex: `Horarios`)
   - No formato, selecione **Valores separados por vírgula (.csv)**
3. Clique em **Publicar** e confirme.
4. **Copie o link gerado** — ele será algo como:
   `https://docs.google.com/spreadsheets/d/SEU_ID/pub?gid=...&single=true&output=csv`
5. Repita para as 4 abas: `Horarios`, `Servicos`, `Campanhas`, `Contato`.

> Guarde os 4 links — você vai precisar deles no próximo passo.

---

## PARTE 4 — Atualizar o index.html para ler a planilha

Abra o arquivo `index.html` e localize este trecho (perto do final do arquivo, dentro da tag `<script>`):

```javascript
const DADOS = {
  horarios: [ ... ],
  servicos: [ ... ],
  ...
```

**Substitua TODO o bloco `const DADOS = { ... };`** pelo código abaixo,
colando os 4 links CSV nos lugares indicados:

```javascript
// ================================================================
// CONEXÃO COM GOOGLE PLANILHAS
// Cole os links CSV de cada aba nos campos abaixo
// ================================================================
const CSV_HORARIOS   = "COLE_AQUI_O_LINK_CSV_DA_ABA_Horarios";
const CSV_SERVICOS   = "COLE_AQUI_O_LINK_CSV_DA_ABA_Servicos";
const CSV_CAMPANHAS  = "COLE_AQUI_O_LINK_CSV_DA_ABA_Campanhas";
const CSV_CONTATO    = "COLE_AQUI_O_LINK_CSV_DA_ABA_Contato";

// Converte texto CSV em array de objetos
function parsearCSV(texto) {
  const linhas = texto.trim().split("\n");
  const cabecalho = linhas[0].split(",").map(c => c.replace(/^"|"$/g, "").trim());
  return linhas.slice(1).map(linha => {
    // suporta vírgulas dentro de aspas
    const valores = [];
    let atual = "", dentroAspas = false;
    for (const ch of linha) {
      if (ch === '"') { dentroAspas = !dentroAspas; }
      else if (ch === ',' && !dentroAspas) { valores.push(atual.trim()); atual = ""; }
      else { atual += ch; }
    }
    valores.push(atual.trim());
    return Object.fromEntries(cabecalho.map((c, i) => [c, (valores[i] || "").replace(/^"|"$/g, "")]));
  });
}

async function carregarDados() {
  try {
    const [resH, resS, resC, resCt] = await Promise.all([
      fetch(CSV_HORARIOS),
      fetch(CSV_SERVICOS),
      fetch(CSV_CAMPANHAS),
      fetch(CSV_CONTATO),
    ]);

    const [txtH, txtS, txtC, txtCt] = await Promise.all([
      resH.text(), resS.text(), resC.text(), resCt.text()
    ]);

    const horarios  = parsearCSV(txtH);
    const servicos  = parsearCSV(txtS).map(r => ({ titulo: r.Titulo, desc: r.Descricao }));
    const campanhas = parsearCSV(txtC);

    // Contato: converte array de {Campo, Valor} em objeto
    const contatoArr = parsearCSV(txtCt);
    const contato = {};
    contatoArr.forEach(r => { contato[r.Campo] = r.Valor; });

    return {
      horarios,
      servicos,
      campanhas,
      contato,
      ultimaAtualizacao: new Date().toLocaleDateString("pt-BR"),
    };

  } catch (erro) {
    console.warn("Erro ao carregar planilha, usando dados locais:", erro);
    return null; // vai usar os DADOS_FALLBACK abaixo
  }
}

// Dados de fallback caso a planilha não carregue
const DADOS_FALLBACK = {
  horarios: [
    { Dia: "Segunda-feira", Horario: "07:00 – 11:00", Servico: "Consulta médica", Obs: "Agendamento" },
  ],
  servicos: [
    { titulo: "Consulta médica", desc: "Atendimento clínico geral." },
  ],
  campanhas: [],
  contato: {
    telefone: "(81) 9 9999-9999",
    endereco: "Bairro Nina Liberato, Caruaru – PE",
    horarioFuncionamento: "Segunda a Sexta: 07h às 17h",
    mapsLink: "https://maps.google.com/?q=UBS+Nina+Liberato+Caruaru",
  },
  ultimaAtualizacao: new Date().toLocaleDateString("pt-BR"),
};
```

Depois, **substitua** o bloco de inicialização no final do script:

```javascript
// ANTES (apague isso):
window.addEventListener("load", () => {
  renderizarHorarios();
  renderizarServicos();
  ...
});

// DEPOIS (coloque isso):
window.addEventListener("load", async () => {
  const dados = await carregarDados();
  // usa planilha se carregou, senão usa fallback
  Object.assign(DADOS_FALLBACK, dados || {});
  // redireciona DADOS para o objeto carregado
  window.DADOS = dados || DADOS_FALLBACK;

  renderizarHorarios();
  renderizarServicos();
  renderizarCampanhas();
  renderizarAutocuidado();
  renderizarContato();

  const loading = document.getElementById("loading");
  loading.style.opacity = "0";
  setTimeout(() => loading.style.display = "none", 350);
});
```

> ⚠️ **Atenção:** as funções de renderização (`renderizarHorarios`, etc.) continuam as mesmas.
> Apenas o bloco de dados e a inicialização mudam.

---

## PARTE 5 — Testar e publicar

1. Salve o `index.html` atualizado.
2. Faça o upload no GitHub (substitua o arquivo antigo).
3. O Vercel atualiza o site automaticamente em 1–2 minutos.
4. Acesse o site e verifique se as informações da planilha aparecem.

---

## Como a equipe da UBS atualiza as informações

Depois que tudo estiver configurado, o processo para a equipe é simples:

1. Abrir a planilha **Nina Comunica – Dados** no Google.
2. Editar o dado desejado (ex: mudar um horário, adicionar uma campanha).
3. **Salvar** (Ctrl+S ou automaticamente).
4. O site já reflete a mudança em segundos — sem precisar mexer no código.

---

## Dicas importantes

- **Não altere os nomes das abas** (`Horarios`, `Servicos`, `Campanhas`, `Contato`) — o código usa esses nomes exatos.
- **Não altere os nomes das colunas** na primeira linha de cada aba.
- Para **adicionar uma campanha**, basta inserir uma nova linha na aba `Campanhas`.
- Para **remover um serviço**, basta apagar a linha correspondente na aba `Servicos`.
- Compartilhe a planilha apenas com os funcionários responsáveis pela atualização (**Acesso restrito**). A publicação como CSV é separada do compartilhamento.
