# AI Model Catalog - Class Diagram

## Updated Class Diagram (Reflecting Final Implementation)

```mermaid
classDiagram
    %% Core Interfaces
    class Metric {
        <<abstract>>
        +score(model_data: dict) float
    }
    
    class BaseHandler {
        <<abstract>>
        +fetch_data() Dict[str, Any]
        +format_data(data: Dict[str, Any]) Dict[str, Any]
        +display_data(formatted_data: Dict[str, Any], raw_data: Dict[str, Any]) None
    }
    
    %% Data Structures
    class MetricResult {
        +name: str
        +score: float
        +passed: bool
        +details: Mapping[str, Any]
        +error: Optional[str]
        +elapsed_s: float
    }
    
    %% Metric Implementations
    class SizeMetric {
        +score(model_data: dict) Dict[str, float]
        -_get_default_score(repo_size_bytes: int, max_size: int) float
    }
    
    class LicenseMetric {
        +score(model_data: dict) float
        -_check_lgplv21_license(license_text: str) bool
    }
    
    class BusFactorMetric {
        +score(model_data: dict) float
        -_calculate_maturity_factor(model_data: dict) float
    }
    
    class RampUpMetric {
        +score(model_data: dict) float
        -_calculate_readme_score(readme: str) float
    }
    
    class PerformanceClaimsMetric {
        +score(model_data: dict) float
        -_contains_performance_indicators(readme: str) bool
    }
    
    class AvailableDatasetAndCodeMetric {
        +score(model_data: dict) float
        -_evaluate_availability(model_data: dict) float
    }
    
    class DatasetQualityMetric {
        +score(model_data: dict) float
        -_analyze_dataset_quality(model_data: dict) float
    }
    
    class CodeQualityMetric {
        +score(model_data: dict) float
        -_analyze_code_quality(model_data: dict) float
    }
    
    %% LLM-Enhanced Metrics
    class LLMEnhancedMetric {
        <<abstract>>
        +score(model_data: dict) float
        +score_with_llm(model_data: dict) float
        +score_traditional(model_data: dict) float
    }
    
    class LLMCodeQualityMetric {
        +score(model_data: dict) float
        +score_with_llm(model_data: dict) float
    }
    
    class LLMDatasetQualityMetric {
        +score(model_data: dict) float
        +score_with_llm(model_data: dict) float
    }
    
    %% Handler Implementations
    class RepositoryHandler {
        -owner: str
        -repo: str
        +fetch_data() Dict[str, Any]
        +format_data(data: Dict[str, Any]) Dict[str, Any]
        +display_data(formatted_data: Dict[str, Any], raw_data: Dict[str, Any]) None
    }
    
    class ModelHandler {
        -model_id: str
        +fetch_data() Dict[str, Any]
        +format_data(data: Dict[str, Any]) Dict[str, Any]
        +display_data(formatted_data: Dict[str, Any], raw_data: Dict[str, Any]) None
    }
    
    %% Service Classes
    class LLMService {
        -api_key: str
        -rate_limit_delay: float
        -last_request_time: float
        -cache: Dict[str, Any]
        +analyze_code_quality(content: str) Optional[Dict[str, Any]]
        +analyze_dataset_quality(content: str) Optional[Dict[str, Any]]
        -_call_api(prompt: str, content: str) Optional[Dict[str, Any]]
        -_rate_limit() None
    }
    
    class LLMServiceSingleton {
        <<singleton>>
        -_instance: LLMService
        +get_instance() LLMService
    }
    
    %% Utility Classes
    class MetricRunner {
        +run_metrics(metrics: Iterable[Metric], ctx, max_workers: int) List[MetricResult]
        +print_ndjson(results: List[MetricResult], stream: TextIO) None
        -_run_one(m: Metric) MetricResult
    }
    
    %% Exception Classes
    class GitHubAPIError {
        +message: str
    }
    
    class RepositoryDataError {
        +message: str
    }
    
    %% Relationships
    Metric <|-- SizeMetric
    Metric <|-- LicenseMetric
    Metric <|-- BusFactorMetric
    Metric <|-- RampUpMetric
    Metric <|-- PerformanceClaimsMetric
    Metric <|-- AvailableDatasetAndCodeMetric
    Metric <|-- DatasetQualityMetric
    Metric <|-- CodeQualityMetric
    
    Metric <|-- LLMEnhancedMetric
    LLMEnhancedMetric <|-- LLMCodeQualityMetric
    LLMEnhancedMetric <|-- LLMDatasetQualityMetric
    
    BaseHandler <|-- RepositoryHandler
    BaseHandler <|-- ModelHandler
    
    LLMServiceSingleton --> LLMService : creates
    LLMCodeQualityMetric --> LLMService : uses
    LLMDatasetQualityMetric --> LLMService : uses
    
    MetricRunner --> Metric : executes
    MetricRunner --> MetricResult : creates
    
    RepositoryHandler --> GitHubAPIError : raises
    ModelHandler --> RepositoryDataError : raises
    
    %% Styling
    classDef interface fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef implementation fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef service fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef exception fill:#ffebee,stroke:#c62828,stroke-width:2px
    classDef data fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
    
    class Metric,BaseHandler interface
    class SizeMetric,LicenseMetric,BusFactorMetric,RampUpMetric,PerformanceClaimsMetric,AvailableDatasetAndCodeMetric,DatasetQualityMetric,CodeQualityMetric,LLMEnhancedMetric,LLMCodeQualityMetric,LLMDatasetQualityMetric,RepositoryHandler,ModelHandler implementation
    class LLMService,LLMServiceSingleton,MetricRunner service
    class GitHubAPIError,RepositoryDataError exception
    class MetricResult data
```

## Key Changes from Initial Design:

1. **Added LLM Integration**: LLMService and LLM-enhanced metrics for advanced analysis
2. **Enhanced Error Handling**: Custom exception hierarchy with proper inheritance
3. **Singleton Pattern**: LLMServiceSingleton for resource management
4. **Concurrent Execution**: MetricRunner for parallel metric execution
5. **Comprehensive Metric Coverage**: All 8 required metrics implemented
6. **Data Structure Standardization**: MetricResult for consistent output format
7. **Service Layer**: Separation of concerns with dedicated service classes
