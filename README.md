# Calculadora HOST

Simulador de rentabilidade em locação por temporada da HOST Gestão Imobiliária, com comparativo ao aluguel tradicional e exportação em PDF.

Site estático: não tem build, banco de dados nem variáveis de ambiente.

## Arquivos

- `index.html`: a calculadora completa (layout, cálculos e PDF)
- `vendor/jspdf.umd.min.js`: gerador de PDF (jsPDF 2.5.1), incluído localmente
- `assets/`: logo oficial da HOST (original, recorte usado na página e versão quadrada)
- `favicon-32.png` e `apple-touch-icon.png`: ícones da aba e do celular, feitos a partir da logo oficial
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
| Comissão HOST, Modelo 1 (valor bruto) | % × diárias |
| Comissão HOST, Modelo 2 (após plataforma) | % × diárias × (1 − % plataforma) |
| Comissão HOST, Modelo 3 (após plataforma, limpeza e custos) | % × (bruta − plataforma − limpeza repassada − custos do imóvel), nunca negativa |
| Custos do imóvel | condomínio + IPTU + energia + internet + outros custos |
| Limpeza repassada | valor bruto da taxa de limpeza, se a opção estiver marcada |
| Receita líquida | bruta − plataforma − HOST − limpeza repassada − custos do imóvel |
| Líquido tradicional | aluguel − % administradora − (aluguel ÷ meses de contrato) |
| Rentabilidade anual | líquido mensal × 12 ÷ valor do imóvel |

Em nenhum modelo a comissão da HOST incide sobre a taxa de limpeza repassada. No aluguel tradicional, condomínio, IPTU, energia e internet ficam com o inquilino e não são descontados. A plataforma cobra sobre o total pago pelo hóspede, então a parte dela sobre a limpeza aparece destacada na composição.

Com as premissas de referência (diária R$ 240, 25 noites, 10 reservas, limpeza R$ 160, plataforma 18%, custos do imóvel R$ 700 (condomínio 300, IPTU 80, energia 180, internet 100, outros 40), HOST 15% no Modelo 2, aluguel R$ 2.000, administradora 10%, 18 meses, imóvel R$ 300.000), o resultado é: bruta R$ 7.600,00, líquida R$ 3.194,00, tradicional R$ 1.688,89, diferença +R$ 1.505,11, rentabilidade 12,78% contra 6,76% ao ano. No Modelo 1 a líquida é R$ 3.032,00 e no Modelo 3, R$ 3.342,20.

## Observações

- Os valores digitados ficam salvos só no navegador de quem usa.
- O PDF é baixado direto pelo navegador e traz imóvel, despesas, modelo de comissionamento e resultados. Em alguns celulares, o navegador abre o PDF antes de oferecer para salvar.
- A fonte Poppins vem do Google Fonts. Sem internet, o navegador usa uma fonte parecida.
- A logo aparece no topo da página e no cabeçalho e rodapé do PDF. Ela está embutida no `index.html`, então o PDF sai com a logo mesmo sem acesso a outros arquivos. Para trocar a logo, substitua a imagem `data:image/png` do elemento `logoHost` no `index.html`.
