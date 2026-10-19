# Calculadora HOST

Simulador de rentabilidade em locação por temporada da HOST Gestão Imobiliária, com comparativo ao aluguel tradicional e exportação em PDF.

Site estático: não tem build, banco de dados nem variáveis de ambiente.

## Arquivos

- `index.html`: a calculadora completa (layout, cálculos e PDF)
- `vendor/jspdf.umd.min.js`: gerador de PDF (jsPDF 2.5.1), incluído localmente
- `favicon.svg`: ícone da aba
- `vercel.json`: cabeçalhos de segurança e cache

## Publicar na Vercel

**Opção 1, pelo site (sem instalar nada)**

1. Crie um repositório no GitHub e envie esta pasta inteira.
2. Na Vercel, clique em **Add New → Project** e importe o repositório.
3. Em **Framework Preset**, escolha **Other**. Deixe **Build Command** vazio e **Output Directory** como está (raiz).
4. Clique em **Deploy**.

**Opção 2, pelo terminal**

```bash
cd host-calculadora
npx vercel        # primeira vez: faz login e cria o projeto
npx vercel --prod # publica em produção
```

Para usar um domínio próprio, como `calculadora.seudominio.com.br`, vá em **Settings → Domains** no projeto da Vercel.

## Regras de cálculo

Mês considerado com 30 dias.

| Item | Fórmula |
|---|---|
| Receita de diárias | diária média × noites ocupadas (máx. 30) |
| Receita de limpeza | taxa de limpeza × reservas (no máximo uma por noite) |
| Receita bruta | diárias + limpeza |
| Comissão da plataforma | % × receita bruta (uma única vez) |
| Comissão da HOST, pós plataforma | % × diárias × (1 − % plataforma) |
| Comissão da HOST, sobre diárias brutas | % × diárias |
| Limpeza repassada | valor bruto da taxa de limpeza, se a opção estiver marcada |
| Receita líquida | bruta − plataforma − HOST − limpeza repassada − custos fixos |
| Líquido tradicional | aluguel − % administradora − (aluguel ÷ meses de contrato) |
| Rentabilidade anual | líquido mensal × 12 ÷ valor do imóvel |

A comissão da HOST nunca incide sobre a taxa de limpeza. A plataforma cobra sobre o total pago pelo hóspede, então a parte dela sobre a limpeza aparece destacada na composição.

Com as premissas de referência (diária R$ 240, 25 noites, 10 reservas, limpeza R$ 160, plataforma 18%, custos R$ 700, HOST 15% pós plataforma, aluguel R$ 2.000, administradora 10%, 18 meses, imóvel R$ 300.000), o resultado é: bruta R$ 7.600,00, líquida R$ 3.194,00, tradicional R$ 1.688,89, diferença +R$ 1.505,11, rentabilidade 12,78% contra 6,76% ao ano.

## Observações

- Os valores digitados ficam salvos só no navegador de quem usa.
- O PDF é baixado direto pelo navegador. Em alguns celulares, o navegador abre o PDF antes de oferecer para salvar.
- A fonte Poppins vem do Google Fonts. Sem internet, o navegador usa uma fonte parecida.
