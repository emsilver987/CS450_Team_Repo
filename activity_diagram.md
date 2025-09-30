# AI Model Catalog - Activity Diagram

## Updated Activity Diagram (Reflecting Final Implementation)

```mermaid
flowchart TD
    A[User Input] --> B{Input Type}
    
    B -->|CLI Command| C[CLI Module]
    B -->|Interactive Mode| D[Interactive Module]
    B -->|Multiple URLs| E[Multiple URLs Handler]
    
    C --> F{Command Type}
    F -->|models| G[GitHub Repository Handler]
    F -->|hf-model| H[Hugging Face Model Handler]
    F -->|hf-dataset| I[Hugging Face Dataset Handler]
    
    G --> J[Fetch GitHub Data]
    H --> K[Fetch HF Model Data]
    I --> L[Fetch HF Dataset Data]
    
    J --> M[Format Repository Data]
    K --> N[Format Model Data]
    L --> O[Format Dataset Data]
    
    M --> P[Display Repository Info]
    N --> Q{Output Format}
    O --> R[Display Dataset Info]
    
    Q -->|text| S[Display Model Info]
    Q -->|ndjson| T[Score Model]
    
    T --> U[Fetch Model Data]
    U --> V[Analyze Local Repository]
    V --> W[Run All Metrics]
    
    W --> X[Size Metric]
    W --> Y[License Metric]
    W --> Z[Bus Factor Metric]
    W --> AA[Ramp Up Time Metric]
    W --> BB[Performance Claims Metric]
    W --> CC[Available Dataset & Code Metric]
    W --> DD[Dataset Quality Metric]
    W --> EE[Code Quality Metric]
    
    X --> FF[Calculate Net Score]
    Y --> FF
    Z --> FF
    AA --> FF
    BB --> FF
    CC --> FF
    DD --> FF
    EE --> FF
    
    FF --> GG[Format NDJSON Output]
    GG --> HH[Display Results]
    
    D --> II[Display Menu]
    II --> JJ{User Choice}
    JJ -->|GitHub| KK[GitHub Repository Browser]
    JJ -->|Hugging Face| LL[Hugging Face Model Browser]
    JJ -->|Exit| MM[Exit Application]
    
    KK --> NN[Select Owner]
    NN --> OO[Select Repository]
    OO --> PP[Fetch & Display Repository]
    
    LL --> QQ[Enter Model ID]
    QQ --> RR[Fetch & Display Model]
    
    E --> SS[Read URL File]
    SS --> TT[Process Each URL]
    TT --> UU{URL Type}
    UU -->|GitHub| VV[Process GitHub URL]
    UU -->|Hugging Face| WW[Process HF URL]
    VV --> XX[Score Repository]
    WW --> YY[Score Model]
    XX --> ZZ[Output NDJSON]
    YY --> ZZ
    
    style A fill:#e1f5fe
    style FF fill:#f3e5f5
    style W fill:#fff3e0
    style ZZ fill:#e8f5e8
```

## Key Changes from Initial Design:

1. **Added Local Repository Analysis**: GitPython integration for cloning and analyzing HF repositories
2. **Enhanced Error Handling**: Comprehensive error isolation and graceful degradation
3. **Multiple Output Formats**: Support for both text and NDJSON output formats
4. **Rate Limiting**: Token-based API rate limiting with fallback mechanisms
5. **Concurrent Metric Execution**: Parallel execution of scoring metrics for performance
6. **Interactive Mode**: Enhanced interactive browsing capabilities
7. **Multiple URL Processing**: Batch processing of multiple URLs from file input
