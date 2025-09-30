# Model Extraction Diagrams

## Diagram 1: Extracted Class Diagram (Generated from Codebase)

```mermaid
classDiagram
    class GitHubAPIError {
        +message: str
    }
    class RepositoryDataError {
        +message: str
    }
    class LLMService {
        -api_key: str
        -rate_limit_delay: float
        -last_request_time: float
        -cache: Dict[str, Any]
        +__init__()
        +analyze_readme_quality()
        +analyze_code_quality_indicators()
        +analyze_dataset_quality()
        -_call_api()
        -_rate_limit()
    }
    class LLMServiceSingleton {
        -_instance: LLMService
        +get_instance()
    }
    class Metric {
        <<abstract>>
        +score(model_data: dict) float
    }
    class LLMEnhancedMetric {
        <<abstract>>
        -llm_service: LLMService
        +__init__()
        +score_with_llm()
        +score_without_llm()
        +score()
    }
    class AvailableDatasetAndCodeMetric {
        +score(model_data: dict) float
        -_evaluate_availability()
    }
    class BusFactorMetric {
        +score(model_data: dict) float
        -_calculate_maturity_factor()
    }
    class CodeQualityMetric {
        +score(model_data: dict) float
        -_analyze_code_quality()
    }
    class LLMCodeQualityMetric {
        +score_with_llm()
        +score_without_llm()
    }
    class DatasetQualityMetric {
        +score(model_data: dict) float
        -_analyze_dataset_quality()
    }
    class LLMDatasetQualityMetric {
        +score_with_llm()
        +score_without_llm()
    }
    class LicenseMetric {
        +score(model_data: dict) float
        -_check_lgplv21_license()
    }
    class PerformanceClaimsMetric {
        +score(model_data: dict) float
        -_contains_performance_indicators()
    }
    class RampUpMetric {
        +score(model_data: dict) float
        -_calculate_readme_score()
    }
    class LLMRampUpMetric {
        +score_with_llm()
        +score_without_llm()
    }
    class SizeMetric {
        +score(model_data: dict) Dict[str, float]
        -_get_default_score()
    }
    class MetricResult {
        +name: str
        +score: float
        +passed: bool
        +details: Mapping[str, Any]
        +error: Optional[str]
        +elapsed_s: float
    }
    class BaseHandler {
        <<abstract>>
        +fetch_data() Dict[str, Any]
        +format_data() Dict[str, Any]
        +display_data() None
    }
    class RepositoryHandler {
        -owner: str
        -repo: str
        +fetch_data()
        +format_data()
        +display_data()
    }
    class ModelHandler {
        -model_id: str
        +fetch_data()
        +format_data()
        +display_data()
    }
    
    Exception <|-- GitHubAPIError
    Exception <|-- RepositoryDataError
    ABC <|-- Metric
    ABC <|-- LLMEnhancedMetric
    Metric <|-- AvailableDatasetAndCodeMetric
    Metric <|-- BusFactorMetric
    Metric <|-- CodeQualityMetric
    LLMEnhancedMetric <|-- LLMCodeQualityMetric
    Metric <|-- DatasetQualityMetric
    LLMEnhancedMetric <|-- LLMDatasetQualityMetric
    Metric <|-- LicenseMetric
    Metric <|-- PerformanceClaimsMetric
    Metric <|-- RampUpMetric
    LLMEnhancedMetric <|-- LLMRampUpMetric
    Metric <|-- SizeMetric
    ABC <|-- BaseHandler
    BaseHandler <|-- RepositoryHandler
    BaseHandler <|-- ModelHandler
```

### Annotations for Extracted Class Diagram:

1. **Abstract Classes**: `Metric`, `LLMEnhancedMetric`, and `BaseHandler` are abstract base classes defining interfaces
2. **Exception Hierarchy**: Custom exceptions inherit from Python's built-in `Exception` class
3. **LLM Integration**: `LLMService` and `LLMServiceSingleton` provide LLM functionality with singleton pattern
4. **Metric Implementations**: All 8 required metrics implemented with both traditional and LLM-enhanced versions
5. **Data Handlers**: `RepositoryHandler` and `ModelHandler` implement the `BaseHandler` interface
6. **Data Structure**: `MetricResult` provides standardized output format for all metrics

## Diagram 2: Extracted Dependency Graph (Generated from Codebase)

```mermaid
graph TD
    subgraph "Core Modules"
        subgraph fetch_repo
            GitHubAPIError
            RepositoryDataError
        end
        subgraph llm_service
            LLMService
            LLMServiceSingleton
        end
    end
    
    subgraph "Metrics System"
        subgraph metrics.base
            Metric
        end
        subgraph metrics.llm_base
            LLMEnhancedMetric
        end
        subgraph "Individual Metrics"
            subgraph metrics.score_available_dataset_and_code
                AvailableDatasetAndCodeMetric
            end
            subgraph metrics.score_bus_factor
                BusFactorMetric
            end
            subgraph metrics.score_code_quality
                CodeQualityMetric
                LLMCodeQualityMetric
            end
            subgraph metrics.score_dataset_quality
                DatasetQualityMetric
                LLMDatasetQualityMetric
            end
            subgraph metrics.score_license
                LicenseMetric
            end
            subgraph metrics.score_performance_claims
                PerformanceClaimsMetric
            end
            subgraph metrics.score_ramp_up_time
                RampUpMetric
                LLMRampUpMetric
            end
            subgraph metrics.score_size
                SizeMetric
            end
            subgraph metrics.types
                MetricResult
            end
        end
    end
    
    subgraph "Data Sources"
        subgraph model_sources.base
            BaseHandler
        end
        subgraph model_sources.github_model
            RepositoryHandler
        end
        subgraph model_sources.hf_model
            ModelHandler
        end
    end
    
    subgraph "Application Layer"
        cli
        interactive
        score_model
    end
    
    %% Dependencies
    cli --> fetch_repo
    cli --> model_sources
    interactive --> fetch_repo
    interactive --> model_sources
    score_model --> fetch_repo
    score_model --> metrics
    metrics.llm_base --> llm_service
    
    %% Styling
    classDef core fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef metrics fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef handlers fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef app fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
    
    class fetch_repo,llm_service core
    class metrics.base,metrics.llm_base,metrics.score_available_dataset_and_code,metrics.score_bus_factor,metrics.score_code_quality,metrics.score_dataset_quality,metrics.score_license,metrics.score_performance_claims,metrics.score_ramp_up_time,metrics.score_size,metrics.types metrics
    class model_sources.base,model_sources.github_model,model_sources.hf_model handlers
    class cli,interactive,score_model app
```

### Annotations for Extracted Dependency Graph:

1. **Layered Architecture**: Clear separation between core modules, metrics system, data sources, and application layer
2. **Dependency Direction**: Dependencies flow inward, with application layer depending on lower layers
3. **LLM Integration**: LLM service is used by LLM-enhanced metrics, showing proper separation of concerns
4. **Modular Design**: Each metric is in its own module, allowing independent development and testing
5. **Data Source Abstraction**: Handlers abstract different data sources (GitHub, Hugging Face) behind common interface

## Summary of Model Extraction Results:

- **Total Classes Found**: 21 classes
- **Modules Analyzed**: 16 modules
- **Inheritance Relationships**: 18 relationships
- **Key Patterns Identified**:
  - Abstract base classes for interfaces
  - Singleton pattern for LLM service
  - Strategy pattern for different metric implementations
  - Factory pattern for metric creation
  - Observer pattern for error handling

## Comparison with Initial Design:

### Changes from Initial Design:
1. **Added LLM Integration**: New `LLMService` and LLM-enhanced metrics
2. **Enhanced Error Handling**: Custom exception hierarchy
3. **Concurrent Execution**: `MetricRunner` for parallel processing
4. **Local Repository Analysis**: GitPython integration for HF repositories
5. **Rate Limiting**: Token-based API rate limiting
6. **Multiple Output Formats**: Support for both text and NDJSON

### Validation of Design Principles:
- **Single Responsibility**: Each class has a focused purpose
- **Open/Closed**: Easy to add new metrics without modifying existing code
- **Dependency Inversion**: High-level modules don't depend on low-level modules
- **Interface Segregation**: Small, focused interfaces
- **Liskov Substitution**: All metric implementations are substitutable
