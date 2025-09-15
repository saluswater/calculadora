# SALUS ROI Calculator

Uma aplicação web Progressive Web App (PWA) para calcular o retorno sobre investimento (ROI) ao utilizar o sistema de purificação de água SALUS em comparação com gastos tradicionais de água engarrafada, produtos de limpeza e consertos de eletrodomésticos.

## 📱 Características

- **PWA Completo**: Funciona offline e pode ser instalado como app nativo
- **Responsivo**: Otimizado para mobile, tablet e desktop
- **Multilíngue**: Suporte para Português e Inglês
- **Modo Escuro**: Alternância entre temas claro e escuro
- **Calculadora Interativa**: Sliders dinâmicos para entrada de dados
- **Cálculos em Tempo Real**: Atualização instantânea dos resultados

## 🎯 Funcionalidades

### Calculadora de ROI
- **Entrada de Dados**: Três sliders para definir custos:
  - Gasto mensal com água engarrafada ($50 - $300)
  - Produtos de limpeza mensais ($30 - $300)
  - Consertos anuais de eletrodomésticos ($200 - $2,000)

### Resultados Calculados
- **Economia Total**: Projeção de economia em 10 anos
- **Economia Mensal**: Valor economizado por mês
- **Economia Anual**: Valor economizado por ano
- **Breakdown Detalhado**: Comparação entre custos atuais e sistema SALUS

### Interface
- **Design Moderno**: Interface limpa e intuitiva
- **Animações Suaves**: Transições e efeitos visuais
- **Controles Acessíveis**: Botões de toggle para idioma e tema
- **Feedback Visual**: Destaque de valores importantes

## 🛠️ Tecnologias Utilizadas

- **Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **PWA**: Service Worker para cache e funcionamento offline
- **Responsividade**: CSS Grid e Flexbox
- **Compatibilidade**: iOS Safari, Chrome, Firefox
- **Persistência**: LocalStorage para preferências do usuário

## 🏗️ Estrutura do Projeto

```
calculadora-salus/
├── index.html          # Aplicação principal
├── manifest.json       # Configurações PWA
├── sw.js              # Service Worker
├── icon.webp          # Ícone da aplicação
├── profit.png         # Favicon
└── README.md          # Documentação
```

## 📊 Lógica de Cálculo

### Custos do Sistema SALUS
- **Custo Mensal**: $75/mês
- **Manutenção Anual**: $96/ano
- **Custo Anual Total**: $996

### Fórmula de Cálculo
```javascript
custoAnualAtual = (águaMensal * 12) + (limpezaMensal * 12) + consertosAnuais
custoAnualSALUS = (75 * 12) + 96
economiaAnual = custoAnualAtual - custoAnualSALUS
economiaMensal = economiaAnual / 12
economia10Anos = economiaAnual * 10
```

## 🎨 Design System

### Cores
- **SALUS Blue**: `#00B4FF` (cor primária da marca)
- **SALUS Red**: `#D81F17` (cor secundária)
- **Backgrounds**: Branco/Preto com variações
- **Text**: Cores adaptáveis ao tema

### Tipografia
- **Primary Font**: SF Pro Display (iOS), BlinkMacSystemFont (macOS), sistema padrão
- **Weights**: 200 (ultra-light), 300 (light), 400 (regular), 500 (medium), 600 (semibold)
- **Logo**: Weight 300 com letter-spacing de 12px

### Breakpoints
- **Mobile**: < 768px
- **Tablet**: 768px - 1023px
- **Desktop**: 1024px - 1199px
- **Large Desktop**: 1200px+
- **Ultra-wide**: 1440px+

## 🌍 Internacionalização

### Idiomas Suportados
- **Português (PT)**: Idioma padrão
- **Inglês (EN)**: Tradução completa

### Sistema de Tradução
- Objeto `translations` com chaves por idioma
- Função `updateLanguage()` para aplicar traduções
- Persistência da preferência no LocalStorage

## 💾 Funcionalidades PWA

### Service Worker
- **Cache Strategy**: Cache-first com fallback para network
- **Recursos Cached**: HTML, CSS, JS, imagens, manifest
- **Versionamento**: `salus-calculator-v1`
- **Cleanup**: Remoção automática de caches antigos

### Manifest
- **Display Mode**: Standalone (funciona como app nativo)
- **Orientação**: Portrait-primary
- **Ícones**: 192x192 e 512x512 em formato WebP
- **Screenshots**: Preview da aplicação
- **Categorias**: Business, Productivity

## 📱 Compatibilidade Mobile

### iOS Safari
- **Safe Area**: Suporte completo para notch e home indicator
- **Touch Targets**: Tamanhos otimizados para toque
- **Viewport**: Configuração específica para iOS
- **Splash Screen**: Tela de carregamento customizada

### Android
- **Theme Color**: Configuração de cor da barra de status
- **Viewport Fit**: Suporte para telas edge-to-edge
- **Touch Actions**: Otimizações para performance

## 🚀 Instalação e Uso

### Requisitos
- Servidor web (HTTP/HTTPS)
- Navegador moderno com suporte a PWA

### Deploy
1. Faça upload de todos os arquivos para o servidor
2. Configure HTTPS (obrigatório para PWA)
3. Acesse a aplicação pelo navegador
4. Instale como PWA quando solicitado

### Desenvolvimento Local
```bash
# Serve com servidor local (Python)
python -m http.server 8000

# Ou com Node.js
npx serve .

# Ou com PHP
php -S localhost:8000
```

## 🔧 Configurações

### Personalização de Valores SALUS
Para alterar os custos do sistema SALUS, edite as variáveis em `index.html`:
```javascript
var salusMonthlyCost = 75;        // Custo mensal em USD
var salusAnnualMaintenance = 96;  // Manutenção anual em USD
```

### Limites dos Sliders
Os valores mínimos e máximos podem ser ajustados nos elementos `<input type="range">`:
```html
<input type="range" min="50" max="300" value="150" id="water-expense">
```

## 🎯 Casos de Uso

### Vendas B2B
- Demonstração de ROI para clientes empresariais
- Cálculos personalizados baseados no perfil do cliente
- Justificativa financeira para investimento em SALUS

### Marketing Digital
- Landing page para campanhas de marketing
- Ferramenta interativa para engajamento
- Geração de leads qualificados

### Apresentações Comerciais
- Suporte visual para equipe de vendas
- Cálculos instantâneos durante negociações
- Projeções financeiras personalizadas

## 📈 Métricas e Analytics

### Eventos Rastreáveis
- Mudanças nos sliders de entrada
- Alternância de idioma
- Mudança de tema
- Instalação do PWA

### Performance
- **First Contentful Paint**: Otimizado para < 1.5s
- **Largest Contentful Paint**: < 2.5s
- **Time to Interactive**: < 3s
- **Bundle Size**: Mínimo (inline CSS/JS)

## 🔒 Segurança

### Dados do Usuário
- **Sem coleta de dados pessoais**
- **Processamento local**: Todos os cálculos executados no cliente
- **LocalStorage apenas**: Para preferências de UI (tema/idioma)

### Privacidade
- Não há comunicação com servidores externos
- Não há tracking ou analytics por padrão
- Funcionamento completamente offline

## 🐛 Troubleshooting

### Problemas Comuns
1. **Calculator não inicializa**: Verifique console para erros de JavaScript
2. **PWA não instala**: Confirme HTTPS e manifest.json válido
3. **Sliders não respondem**: Problema de compatibilidade - fallback automático ativo
4. **Tema não persiste**: LocalStorage bloqueado ou em modo privado

### Compatibilidade de Navegadores
- **Chrome/Edge**: Suporte completo
- **Safari**: Suporte completo com fallbacks
- **Firefox**: Suporte completo
- **iOS Safari**: Otimizações específicas implementadas
- **Android WebView**: Testado e funcional

## 📝 Changelog

### v1.0.0
- Aplicação inicial com calculadora de ROI
- Suporte PWA completo
- Multilíngue (PT/EN)
- Modo escuro
- Responsividade completa
- Compatibilidade iOS Safari

## 🤝 Contribuição

### Para Desenvolvedores
1. Clone o repositório
2. Faça suas alterações
3. Teste em múltiplos dispositivos e navegadores
4. Mantenha compatibilidade PWA
5. Submeta pull request

### Testes Recomendados
- **Desktop**: Chrome, Firefox, Safari, Edge
- **Mobile**: iOS Safari, Chrome Mobile, Samsung Internet
- **PWA**: Instalação e funcionamento offline
- **Responsividade**: Diferentes tamanhos de tela

## 📄 Licença

Propriedade da SALUS - Sistema de Purificação de Água.

---

**SALUS** - Tecnologia de Purificação de Água  
Economia inteligente, água pura, vida saudável.