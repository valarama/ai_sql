# ai_sql
Advanced system for converting English data quality rules into validated SQL using LangGraph, Chroma, and Neo4j

# Advanced Dynamic SQL Rule Generator System

A comprehensive three-stage system for converting English data quality rules into validated SQL conditions using LangGraph, Chroma, Neo4j, and advanced validation techniques.

## 🏗️ System Architecture

### **Stage 1: Pattern Analysis & Rule Storage**
- **Chroma**: Vector database for rule embeddings and similarity search
- **Neo4j**: Graph database for relationship tracking
- **Pattern Analysis**: Dynamic data pattern detection without hardcoded assumptions
- **Output**: Individual rule JSON files (`rule_column_abc123.json`)

### **Stage 2: Intelligent SQL Generation with Retrieval**
- **Retrieval-Augmented Generation**: Uses Chroma to find similar successful rules
- **LangGraph**: Stateful workflow management with retry logic
- **Dynamic Prompts**: Jinja2 templates for context-aware SQL generation
- **Sample Data**: Uses up to 3 records for improved pattern understanding

### **Stage 3: Advanced Validation & Quality Assessment**
- **HybridRAG Validation**: Multi-dimensional validation approach
- **Syntax Validation**: SQL parsing and structure verification
- **Semantic Validation**: Rule-to-SQL alignment checking
- **Test Case Validation**: Application to sample data
- **Quality Scoring**: Comprehensive 0-100 quality metrics

## 🚀 Quick Start

### 1. Setup System
```bash
# Install dependencies
pip install -r requirements.txt

# Run complete setup and demo
python setup_and_demo.py

# Or setup only
python setup_and_demo.py --setup-only
```

### 2. Configure Environment
Edit `.env` file for your LLM provider:

**For Local LLM:**
```bash
LLM_PROVIDER=local
LOCAL_LLM_API_URL=http://localhost:1138/v1/completions
```

**For GCP Gemini:**
```bash
LLM_PROVIDER=gcp
GEMINI_API_KEY=your_api_key_here
GEMINI_MODEL=gemini-2.0-flash
```

### 3. Run Processing
```bash
# Run main system
python advanced_sql_system.py

# Or run demo
python setup_and_demo.py --demo-only
```

## 📁 Folder Structure

```
advanced_sql_system/
├── input/
│   ├── rules/
│   │   ├── rules.csv                    # Input rules
│   │   ├── sample_data.csv              # Sample data
│   │   └── rule_*.json                  # Individual rule files
│   └── data/
├── output/
│   ├── sql/                             # Generated SQL files
│   │   └── *.sql                        # Individual SQL files
│   ├── validation/                      # Validation results
│   │   └── dq_results_report_*.json     # HybridRAG validation
│   ├── summaries/                       # Processing summaries
│   │   └── processing_summary_*.json    # Complete processing reports
│   └── monitoring/                      # MCP monitoring data
├── templates/                           # Jinja2 templates
├── logs/                               # System logs
└── chroma_hybrid_dq_db/                # Chroma vector database
```

## 🔍 Traceability & Monitoring

### **Neo4j Relationships**
```cypher
# View complete pipeline for a column
MATCH (c:Column)-[:HAS_RULE]->(r:Rule)-[:GENERATES]->(s:SQL)-[:VALIDATED_AS]->(v:Validation)
WHERE c.name = 'customer_email'
RETURN c, r, s, v

# Find high-quality SQL generations
MATCH (s:SQL)-[:VALIDATED_AS]->(v:Validation)
WHERE v.quality_score >= 0.85
RETURN s.code, v.quality_score
ORDER BY v.quality_score DESC
```

### **Chroma Retrieval**
- Rule embeddings for similarity search
- Pattern-based retrieval context
- Successful SQL generation history

### **Complete Processing Trace**
Every rule processing includes:
- Stage-by-stage execution timeline
- Quality progression tracking
- Retry attempts and improvements
- Final validation metrics

## 📊 Example Output

### **Generated SQL File**
```sql
-- Rule ID: rule_1_abc12345
-- Column: customer_email
-- Original Rule: Must be valid email format and must not contain noreply
-- Generated SQL Quality Score: 0.92
-- Status: completed_successfully
-- Validation Results: Syntax✅ Semantic✅ HybridRAG✅

customer_email REGEXP '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$' 
AND customer_email NOT LIKE '%noreply%' 
AND customer_email IS NOT NULL
```

### **Validation Results**
```json
{
  "dq_results_summary": {
    "rule_id": "rule_1_abc12345",
    "column_name": "customer_email",
    "overall_quality_score": 0.92,
    "is_valid": true,
    "processing_method": "hybrid_rag_stage3"
  },
  "dq_validation_results": [{
    "validation_details": {
      "syntax_validation": {"valid": true, "score": 1.0},
      "semantic_validation": {"valid": true, "score": 0.95},
      "hybridrag_validation": {"valid": true, "score": 0.85}
    }
  }],
  "traceability": {
    "processing_trace": [...]
  }
}
```

### **Dynamic JSON Summary**
```json
{
  "processing_summary": {
    "system_info": {
      "processing_id": "proc_20241221_143022",
      "timestamp": "2024-12-21T14:30:22",
      "stages_completed": 3
    },
    "stage_results": {
      "stage1_storage": {
        "rules_stored_chroma": 8,
        "rules_stored_neo4j": 8,
        "pattern_analysis_completed": 8
      },
      "stage2_generation": {
        "sql_generated_successfully": 7,
        "retrieval_contexts_used": 6
      },
      "stage3_validation": {
        "validations_completed": 7,
        "average_quality_score": 0.847,
        "high_quality_rules": 5
      }
    },
    "quality_metrics": {
      "overall_success_rate": 87.50,
      "average_quality_score": 0.847
    }
  }
}
```

## ⚙️ Configuration Options

### **Quality Thresholds**
```bash
QUALITY_THRESHOLD=0.85          # Minimum quality for acceptance
MAX_RETRIES=3                   # Maximum retry attempts
```

### **Processing Settings**
```bash
SAMPLE_RECORDS=3                # Sample data records per rule
LLM_MAX_TOKENS=512             # Maximum LLM response tokens
LLM_TEMPERATURE=0.1            # LLM creativity (0.0-1.0)
```

### **Database Configuration**
```bash
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=password
CHROMA_PERSIST_DIR=./chroma_hybrid_dq_db
```

## 🔧 Advanced Features

### **Dynamic Prompt Templates**
- Jinja2-based template system
- Context-aware prompt generation
- Retrieval-augmented prompting
- No hardcoded assumptions

### **Multi-Strategy Validation**
- **Syntax**: SQL parsing validation
- **Semantic**: Rule-SQL alignment checking
- **Performance**: Anti-pattern detection
- **Security**: Injection pattern detection
- **Test Cases**: Sample data application

### **LangGraph Workflow**
- Stateful processing with retry logic
- Conditional routing based on quality
- Automatic improvement with feedback
- Complete execution tracing

### **MCP Monitoring**
- Real-time processing monitoring
- Quality progression tracking
- Performance metrics collection
- Complete audit trails

## 🛠️ Customization

### **Add New Validation Strategy**
1. Extend `Stage3HybridRAGValidator`
2. Add validation method
3. Update quality score calculation
4. Add to Neo4j storage

### **Custom Prompt Templates**
1. Edit `templates/sql_generation.j2`
2. Add new template variables
3. Update `Stage2SQLGenerator`

### **New Data Patterns**
1. Extend `_analyze_data_patterns()`
2. Add pattern detection logic
3. Update Chroma metadata

## 🐛 Troubleshooting

### **Common Issues**

**LLM Connection Failed**
```bash
# Check LLM server status
curl http://localhost:1138/v1/completions

# Verify .env configuration
cat .env | grep LLM
```

**Neo4j Connection Failed**
```bash
# Check Neo4j status
docker ps | grep neo4j

# Test connection
python -c "from neo4j import GraphDatabase; print('Connected' if GraphDatabase.driver('bolt://localhost:7687', auth=('neo4j', 'password')).verify_connectivity() else 'Failed')"
```

**Chroma Database Issues**
```bash
# Clear Chroma database
rm -rf ./chroma_hybrid_dq_db

# Restart system
python advanced_sql_system.py
```

### **Debug Mode**
```bash
# Enable debug logging
export LOG_LEVEL=DEBUG
python advanced_sql_system.py
```

## 🎯 Best Practices

1. **Rule Writing**: Use clear, specific language in rules
2. **Sample Data**: Provide diverse, realistic sample data
3. **Quality Monitoring**: Review generated SQL before production use
4. **Traceability**: Use Neo4j queries for audit trails
5. **Performance**: Monitor LLM response times and quality scores

## 📈 Performance Metrics

- **Processing Speed**: ~2-5 seconds per rule (depending on LLM)
- **Quality Achievement**: 85%+ quality score typical
- **Success Rate**: 90%+ with retry logic
- **Scalability**: Handles 100+ rules efficiently

## 🤝 Contributing

1. Fork the repository
2. Create feature branch
3. Add tests for new functionality
4. Update documentation
5. Submit pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

**🎯 Ready to transform your data quality rules into validated SQL?**
**Start with: `python setup_and_demo.py`**
