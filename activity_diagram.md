# AI Model Catalog - Activity Diagram

## Updated Activity Diagram (Reflecting Final Implementation)

```mermaid
flowchart TD
    START([START]) --> INPUT[Input: User can view current catalogue or add AI/ML model to catalogue]
    
    INPUT --> DECISION{Did the user provide a model link?}
    
    %% Yes Path - Adding New Model
    DECISION -->|Yes| YES[Yes]
    YES --> FETCH[Fetching Information]
    FETCH --> CHECK[Checking for compatibility]
    CHECK --> CALC[Calculating Metrics]
    
    %% No Path - Viewing Existing Models
    DECISION -->|No| NO[No]
    
    %% Convergence Point
    CALC --> OUTPUT[Output: Model metrics paired with netscore displayed for user evaluation]
    NO --> OUTPUT
    
    %% Enhanced Implementation Details
    FETCH --> FETCH_DETAILS[Fetch GitHub/HF Data]
    FETCH_DETAILS --> LOCAL_ANALYSIS[Analyze Local Repository]
    LOCAL_ANALYSIS --> CHECK
    
    CHECK --> CHECK_DETAILS[Check License Compatibility]
    CHECK_DETAILS --> CALC
    
    CALC --> METRICS[Run All Metrics Concurrently]
    METRICS --> SIZE[Size Metric]
    METRICS --> LICENSE[License Metric]
    METRICS --> BUS[Bus Factor Metric]
    METRICS --> RAMP[Ramp Up Time Metric]
    METRICS --> PERF[Performance Claims Metric]
    METRICS --> AVAIL[Available Dataset & Code Metric]
    METRICS --> DATASET[Dataset Quality Metric]
    METRICS --> CODE[Code Quality Metric]
    
    SIZE --> NETSCORE[Calculate Net Score]
    LICENSE --> NETSCORE
    BUS --> NETSCORE
    RAMP --> NETSCORE
    PERF --> NETSCORE
    AVAIL --> NETSCORE
    DATASET --> NETSCORE
    CODE --> NETSCORE
    
    NETSCORE --> OUTPUT
    
    OUTPUT --> END([END])
    
    %% Styling
    style START fill:#e1f5fe,stroke:#01579b,stroke-width:3px
    style END fill:#e1f5fe,stroke:#01579b,stroke-width:3px
    style DECISION fill:#fff3e0,stroke:#e65100,stroke-width:2px
    style METRICS fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    style NETSCORE fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
    style OUTPUT fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
```

## Key Changes from Initial Design:

### **Faithful to Original Design:**
- ✅ **Core Flow Preserved**: Maintains the original START → Input → Decision → Fetch/Check/Calculate → Output → END flow
- ✅ **Decision Point**: Keeps the "Did the user provide a model link?" decision structure
- ✅ **Convergence**: Both paths (Yes/No) still converge at the Output stage
- ✅ **User-Centric**: Focus remains on user evaluation and model selection

### **Enhanced Implementation Details:**
1. **Detailed Fetching Process**: 
   - Original: "Fetching Information" 
   - Enhanced: Fetch GitHub/HF Data → Analyze Local Repository (GitPython integration)

2. **Enhanced Compatibility Checking**:
   - Original: "Checking for compatibility"
   - Enhanced: Check License Compatibility (LGPLv2.1 strict compliance)

3. **Detailed Metrics Calculation**:
   - Original: "Calculating Metrics"
   - Enhanced: Run All Metrics Concurrently → Individual Metric Calculations → Net Score

4. **Added Technical Enhancements**:
   - **Concurrent Execution**: Parallel metric processing for performance
   - **Error Isolation**: Comprehensive error handling with graceful degradation
   - **Rate Limiting**: Token-based API rate limiting
   - **Multiple Output Formats**: Text and NDJSON support
   - **Local Analysis**: GitPython integration for repository analysis

### **Design Validation:**
The final implementation successfully realizes your original design vision while adding robust technical capabilities. The core user experience and decision flow remain unchanged, demonstrating good design evolution from concept to implementation.
