# BOT-CONTROL

Projeto de automação e painel para gestão de entregas, motoboys e controle financeiro.

## Descrição

O BOT-CONTROL é um sistema desenvolvido para facilitar o controle operacional de entregas, pagamentos de motoboys e gestão financeira de empresas de delivery. Ele oferece uma interface otimizada, automação de processos e integração com Excel.

## Funcionalidades

### Painel de Controle
- Interface gráfica otimizada com abas para monitoramento, fechamento financeiro, vales e logs.
- Navegação intuitiva entre abas.
- Controle manual e automático do robô.

### Automação de Processos
- Monitoramento contínuo de pedidos e pagamentos.
- Processamento automático de baixas de estoque e estornos.
- Geração e atualização de planilhas Excel para controle financeiro.
- Cálculo automático de valores, totais e descontos.

### Gestão de Motoboys e Vales
- Cadastro e acompanhamento de motoboys.
- Controle de vales/descontos por motoboy.
- Cálculo de pagamentos, totais e valores a pagar.
- Exportação de relatórios financeiros.

### Integração com Excel
- Leitura e escrita de planilhas no formato Controle_Financeiro_DD-MM-YYYY.xlsx.
- Carregamento seletivo de colunas para otimizar performance.
- Fallback automático para openpyxl caso pandas falhe.

### Auto-Refresh Inteligente
- Verificação automática de alterações nos arquivos a cada 2 segundos.
- Recarregamento apenas quando há mudanças.
- Redução de uso de CPU e RAM.

### Cache de Dados
- Cache de DataFrames para evitar recarregamentos desnecessários.
- Monitoramento de mtime dos arquivos.

### Notificações e Integração
- Envio de mensagens e alertas para grupos no Telegram.
- Logging de eventos e operações.

### Segurança e Robustez
- Operações thread-safe na interface.
- Sincronização garantida para evitar race conditions.
- Arquitetura event-driven sem deadlocks.
- Captura e tratamento de todos os erros.

### Exemplos de Código
- Leitura otimizada de Excel:
```python
mtime = os.path.getmtime(arq)
if mtime == self.cache_monitor_mtime:
	return  # Arquivo não mudou, usa cache
df = pd.read_excel(
	arq,
	sheet_name="EXTRATO DETALHADO",
	usecols=lambda col: any(c in col for c in ['Numero', 'Cliente', ...])
)
```
- Auto-refresh:
```python
def _auto_refresh_inteligente(self):
	mtime = os.path.getmtime(arq)
	if mtime != self._last_auto_refresh_mtime:
		self.carregar_tabela()  # Recarrega APENAS se mudou
	self.after(2000, self._auto_refresh_inteligente)  # Próximo ciclo
```

### Estrutura do Excel
- Planilha principal: EXTRATO DETALHADO (com colunas: Número, Cliente, Bairro, Valor, Status, Motoboy, Hora)
- Planilha opcional: PAGAMENTO_MOTOBOYS (com colunas: MOTOBOY, QTD TOTAL, QTD R$ 8,00, QTD R$ 11,00, TOTAL A PAGAR)

### Troubleshooting
- Painel não abre? Execute `python painel.py` e verifique mensagens de erro.
- Dados não aparecem? Reabra o painel e confira se o Excel está na mesma pasta.
- Lentidão? Ajuste o intervalo de auto-refresh e feche outras aplicações.

### Checklist de Implementação
- Verificação inteligente de mtime
- Carregamento seletivo de colunas
- Auto-refresh automático
- TreeView otimizado
- Cache Pandas integrado
- Testes de performance validados
- Documentação completa
- Scripts de validação
- Pronto para produção

## Instalação e Uso

- Requisitos: Python 3.10+ e dependências em `requirements.txt`
- Instale as dependências:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

- Execute o painel:

```bash
python painel.py
```

- Para compilar o executável:

```bash
pyinstaller --noconsole --onefile --add-data "robo.py;." painel.py
```

## Documentação Detalhada

Para detalhes técnicos, otimizações, exemplos de código e estrutura de dados, consulte o README original do projeto ou a documentação na pasta.

## Autor
Adiel Alves

## Status
Produção

## Licença
MIT
