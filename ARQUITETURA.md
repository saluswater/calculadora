# ARQUITETURA - SALUS ROI Calculator

## 📋 Visão Geral

A **SALUS ROI Calculator** é uma Progressive Web Application (PWA) desenvolvida com arquitetura frontend-only, focada em performance, usabilidade e compatibilidade multiplataforma. A aplicação utiliza tecnologias web nativas (HTML5, CSS3, JavaScript Vanilla) para entregar uma experiência nativa em dispositivos móveis e desktop.

## 🏗️ Arquitetura Geral

### Padrão Arquitetural
**Single Page Application (SPA) com arquitetura Frontend-Only**

```
┌─────────────────────────────────────────────────────────────┐
│                    BROWSER CLIENT                           │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌──────────────┐  ┌────────────────┐  │
│  │   HTML5 DOM     │  │   CSS3 UI    │  │  JavaScript    │  │
│  │   Structure     │  │   Styling    │  │   Logic        │  │
│  └─────────────────┘  └──────────────┘  └────────────────┘  │
├─────────────────────────────────────────────────────────────┤
│              SERVICE WORKER (PWA LAYER)                    │
│  ┌─────────────────┐  ┌──────────────┐  ┌────────────────┐  │
│  │   Cache API     │  │  Fetch API   │  │  Push/Sync     │  │
│  │   Management    │  │  Interceptor │  │  (Future)      │  │
│  └─────────────────┘  └──────────────┘  └────────────────┘  │
├─────────────────────────────────────────────────────────────┤
│              BROWSER APIS & STORAGE                         │
│  ┌─────────────────┐  ┌──────────────┐  ┌────────────────┐  │
│  │  LocalStorage   │  │   IndexedDB  │  │  Web Assembly  │  │
│  │  (Preferences)  │  │   (Future)   │  │    (Future)    │  │
│  └─────────────────┘  └──────────────┘  └────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## 🔧 Componentes da Arquitetura

### 1. Presentation Layer (Camada de Apresentação)

#### Estrutura HTML Semântica
- **Organização**: HTML5 semântico com elementos `<main>`, `<section>`, `<header>`
- **Acessibilidade**: Atributos ARIA, labels apropriados, navegação por teclado
- **SEO**: Meta tags otimizadas, structured data

```html
<!-- Estrutura semântica -->
<div class="app">
    <header class="controls">     <!-- Controles de UI -->
    <main class="main">           <!-- Conteúdo principal -->
        <section class="brand">       <!-- Branding -->
        <section class="calculator">  <!-- Funcionalidade -->
            <div class="input-panel">     <!-- Entradas -->
            <div class="results-panel">   <!-- Resultados -->
    </main>
</div>
```

#### Sistema de CSS Modular
- **Metodologia**: BEM-like naming com CSS custom properties
- **Responsividade**: Mobile-first com media queries progressivas
- **Temas**: CSS custom properties para modo claro/escuro

```css
/* Arquitetura CSS */
:root {
    /* Design tokens */
    --salus-blue: #00B4FF;
    --primary-bg: #ffffff;
    /* ... outros tokens */
}

/* Componentes modulares */
.calculator { /* Layout container */ }
.input-panel { /* Input grouping */ }  
.results-panel { /* Results display */ }
.slider-container { /* Slider wrapper */ }
```

### 2. Business Logic Layer (Camada de Lógica de Negócio)

#### Calculator Engine
```javascript
// Arquitetura de cálculo
var CalculatorEngine = {
    // Constantes do sistema SALUS
    SALUS_MONTHLY_COST: 75,
    SALUS_ANNUAL_MAINTENANCE: 96,
    
    // Métodos de cálculo
    calculateAnnualCost: function(water, cleaning, repair) {
        return (water * 12) + (cleaning * 12) + repair;
    },
    
    calculateSavings: function(currentCost, salusCost) {
        return currentCost - salusCost;
    },
    
    projectTenYearSavings: function(annualSavings) {
        return annualSavings * 10;
    }
};
```

#### State Management
```javascript
// Estado da aplicação
var AppState = {
    // Valores dos sliders
    inputs: {
        waterExpense: 150,
        cleaningExpense: 100,
        repairExpense: 1200
    },
    
    // Configurações de UI
    preferences: {
        language: 'pt',
        theme: 'light'
    },
    
    // Resultados calculados
    results: {
        monthlySavings: 0,
        annualSavings: 0,
        tenYearSavings: 0,
        currentAnnualCost: 0
    }
};
```

### 3. Data Layer (Camada de Dados)

#### LocalStorage Strategy
```javascript
// Estratégia de persistência
var StorageManager = {
    // Chaves de armazenamento
    KEYS: {
        THEME: 'theme',
        LANGUAGE: 'language',
        USER_INPUTS: 'userInputs' // Futuro
    },
    
    // Métodos de persistência
    save: function(key, value) {
        try {
            localStorage.setItem(key, JSON.stringify(value));
        } catch(e) {
            console.log('Storage unavailable');
        }
    },
    
    load: function(key) {
        try {
            return JSON.parse(localStorage.getItem(key));
        } catch(e) {
            return null;
        }
    }
};
```

### 4. Service Worker Layer (Camada PWA)

#### Cache Strategy
```javascript
// sw.js - Estratégia de cache
const CACHE_STRATEGY = {
    // Cache-first para recursos estáticos
    STATIC_RESOURCES: 'cache-first',
    
    // Network-first para dados dinâmicos (futuro)
    DYNAMIC_DATA: 'network-first',
    
    // Stale-while-revalidate para atualizações
    UPDATES: 'stale-while-revalidate'
};

// Lifecycle management
self.addEventListener('install', handleInstall);
self.addEventListener('activate', handleActivate);
self.addEventListener('fetch', handleFetch);
```

## 🎯 Padrões de Design Implementados

### 1. Module Pattern
```javascript
// Módulos autocontidos
var LanguageManager = (function() {
    var currentLang = 'pt';
    var translations = { /* ... */ };
    
    return {
        toggle: function() { /* ... */ },
        update: function() { /* ... */ },
        getCurrent: function() { return currentLang; }
    };
})();
```

### 2. Observer Pattern
```javascript
// Event-driven updates
function initEventListeners() {
    // Sliders observam mudanças e notificam calculadora
    waterSlider.addEventListener('input', function() {
        updateDisplay();
        calculate();
    });
}
```

### 3. Strategy Pattern
```javascript
// Diferentes estratégias de inicialização
var InitStrategies = {
    domReady: function() { /* DOMContentLoaded */ },
    windowLoad: function() { /* window.load */ },
    fallback: function() { /* setTimeout fallback */ },
    emergency: function() { /* Direct manipulation */ }
};
```

## 📱 Arquitetura Responsiva

### Breakpoint Strategy
```css
/* Mobile-first approach */
/* Base: 320px - 767px (Mobile) */

@media screen and (min-width: 768px) {
    /* Tablet: 768px - 1023px */
}

@media screen and (min-width: 1024px) {
    /* Desktop: 1024px - 1199px */
}

@media screen and (min-width: 1200px) {
    /* Large Desktop: 1200px+ */
    .calculator {
        display: grid;
        grid-template-columns: 1fr 500px;
        gap: 80px;
    }
}
```

### Layout Architecture
```css
/* Flexible layout system */
.app {
    display: flex;
    flex-direction: column;
    min-height: 100dvh; /* Dynamic viewport */
}

.main {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

/* Grid system para desktop */
.calculator {
    display: grid;
    grid-template-columns: 1fr;
    gap: 40px;
}
```

## 🔄 Fluxo de Dados

### 1. Input Flow
```
User Interaction → Slider Event → Update Display → Calculate Results → Update UI
```

### 2. State Flow
```javascript
// Fluxo de estado unidirecional
UserInput → StateUpdate → BusinessLogic → ResultCalculation → UIUpdate
```

### 3. Persistence Flow
```
UserAction → StateChange → LocalStorage → NextSession → StateRestore
```

## 🌐 Internacionalização (i18n)

### Translation Architecture
```javascript
// Sistema de tradução centralizado
var i18n = {
    translations: {
        'pt': { /* Portuguese translations */ },
        'en': { /* English translations */ }
    },
    
    currentLang: 'pt',
    
    t: function(key) {
        return this.translations[this.currentLang][key] || key;
    },
    
    setLanguage: function(lang) {
        this.currentLang = lang;
        this.updateDOM();
    }
};
```

### Content Strategy
- **Static Content**: Traduzido via JavaScript
- **Dynamic Content**: Formatação de números e moedas
- **Fallbacks**: Chave exibida se tradução não encontrada

## ⚡ Performance Architecture

### Loading Strategy
```javascript
// Estratégia de carregamento otimizada
var LoadingStrategy = {
    // Critical path: HTML + inline CSS + inline JS
    critical: 'inline',
    
    // Non-critical: Diferido ou lazy-loaded
    nonCritical: 'deferred',
    
    // Progressive enhancement
    enhancement: 'conditional'
};
```

### Bundle Strategy
- **Inline Everything**: Zero HTTP requests após cache
- **Critical CSS**: Inline no `<head>`
- **Critical JS**: Inline antes do `</body>`
- **Assets**: Otimizados (WebP, SVG)

### Memory Management
```javascript
// Gestão de memória
var MemoryManager = {
    // Evitar memory leaks
    cleanupEventListeners: function() {
        // Remove listeners quando necessário
    },
    
    // Throttle/debounce para performance
    throttleCalculations: function() {
        // Limitação de cálculos frequentes
    }
};
```

## 🛡️ Security Architecture

### Client-side Security
```javascript
// Validação de entrada
var InputValidator = {
    validateRange: function(value, min, max) {
        return Math.max(min, Math.min(max, parseFloat(value) || 0));
    },
    
    sanitizeInput: function(input) {
        // Sanitização básica
        return input.toString().replace(/[^\d.-]/g, '');
    }
};
```

### Privacy by Design
- **No External Calls**: Zero comunicação externa
- **Local Processing**: Todos os cálculos no cliente
- **Minimal Storage**: Apenas preferências de UI

## 🔧 Error Handling Architecture

### Graceful Degradation
```javascript
// Sistema de fallbacks múltiplos
var ErrorHandling = {
    // Inicialização com fallbacks
    initWithFallbacks: function() {
        try {
            this.primaryInit();
        } catch(e) {
            try {
                this.secondaryInit();
            } catch(e2) {
                this.emergencyInit();
            }
        }
    },
    
    // Recovery strategies
    recoverFromError: function(error) {
        console.log('Error handled:', error);
        // Implementar recovery
    }
};
```

### Compatibility Layer
```javascript
// Camada de compatibilidade para browsers antigos
var CompatLayer = {
    // Feature detection
    supportsFeature: function(feature) {
        return typeof feature !== 'undefined';
    },
    
    // Polyfills condicionais
    loadPolyfills: function() {
        if (!this.supportsFeature(window.fetch)) {
            // Load fetch polyfill
        }
    }
};
```

## 🔄 PWA Architecture

### Service Worker Lifecycle
```javascript
// Gestão do ciclo de vida PWA
var PWAManager = {
    // Instalação
    install: function() {
        // Cache recursos críticos
        return caches.open(CACHE_NAME)
            .then(cache => cache.addAll(CRITICAL_RESOURCES));
    },
    
    // Ativação
    activate: function() {
        // Limpar caches antigos
        return this.cleanupOldCaches();
    },
    
    // Intercepção de requests
    fetch: function(event) {
        // Cache-first strategy
        return caches.match(event.request)
            .then(response => response || fetch(event.request));
    }
};
```

### Offline Strategy
```javascript
// Estratégia offline
var OfflineStrategy = {
    // Recursos essenciais sempre cached
    criticalResources: [
        './index.html',
        './manifest.json',
        './sw.js'
    ],
    
    // Funcionalidade offline completa
    enableOfflineMode: function() {
        // App funciona 100% offline
        return true;
    }
};
```

## 📊 Testing Architecture

### Testing Strategy (Recommendations)
```javascript
// Estrutura de testes recomendada (não implementada)
var TestSuite = {
    // Unit tests
    calculator: {
        testCalculations: function() {},
        testInputValidation: function() {},
        testStateManagement: function() {}
    },
    
    // Integration tests
    userFlow: {
        testSliderInteraction: function() {},
        testLanguageToggle: function() {},
        testThemeToggle: function() {}
    },
    
    // E2E tests
    crossBrowser: {
        testIOSSafari: function() {},
        testAndroidChrome: function() {},
        testDesktopBrowsers: function() {}
    }
};
```

## 🚀 Deployment Architecture

### Static Hosting Strategy
```yaml
# Configuração de deploy
deployment:
  type: "static-hosting"
  requirements:
    - HTTPS (obrigatório para PWA)
    - Gzip compression
    - Cache headers
    - MIME types corretos
  
  providers:
    - Netlify
    - Vercel  
    - GitHub Pages
    - AWS S3 + CloudFront
```

### Build Process (Future)
```javascript
// Processo de build futuro
var BuildProcess = {
    // Minificação
    minify: {
        html: true,
        css: true, 
        js: true
    },
    
    // Otimização de assets
    optimize: {
        images: true,
        fonts: true,
        icons: true
    },
    
    // Bundle splitting
    chunks: {
        critical: 'inline',
        vendor: 'separate',
        lazy: 'dynamic'
    }
};
```

## 🔮 Future Architecture Considerations

### Scalability Roadmap
1. **Component Library**: Modularização em componentes reutilizáveis
2. **State Management**: Redux/Context API para estado complexo
3. **API Integration**: Backend para analytics e personalização
4. **Micro-frontends**: Divisão em módulos independentes
5. **WebAssembly**: Performance crítica para cálculos complexos

### Technology Evolution
```javascript
// Migração tecnológica futura
var TechRoadmap = {
    // Frameworks modernos
    frontend: ['React', 'Vue', 'Svelte'],
    
    // Build tools
    bundling: ['Vite', 'esbuild', 'Rollup'],
    
    // Testing
    testing: ['Vitest', 'Playwright', 'Cypress'],
    
    // Backend (se necessário)
    backend: ['Node.js', 'Deno', 'Bun']
};
```

## 📈 Monitoring & Analytics Architecture

### Performance Monitoring
```javascript
// Monitoramento de performance (futuro)
var PerformanceMonitor = {
    // Web Vitals
    trackCoreVitals: function() {
        // LCP, FID, CLS tracking
    },
    
    // Custom metrics
    trackUserInteractions: function() {
        // Slider usage, language toggle, etc.
    },
    
    // Error tracking
    trackErrors: function() {
        // Runtime errors, compatibility issues
    }
};
```

## 🎯 Conclusão Arquitetural

A **SALUS ROI Calculator** implementa uma arquitetura frontend-only moderna, focada em:

- **Performance**: Carregamento rápido e responsividade
- **Compatibilidade**: Funciona em todos os dispositivos e navegadores
- **Usabilidade**: Interface intuitiva e acessível
- **Manutenibilidade**: Código organizado e bem estruturado
- **Escalabilidade**: Preparado para futuras expansões

A aplicação demonstra como tecnologias web nativas podem entregar experiências comparáveis a aplicações nativas, mantendo simplicidade e performance através de decisões arquiteturais cuidadosas.

---

**Desenvolvido com foco em engenharia de software de qualidade e melhores práticas da indústria.**