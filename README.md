# Branding-King

A sophisticated agent system that maintains state and context through the combination of persistent entity storage (SQLite) and vector-based memory (ChromaDB).

## Overview

Branding-King is designed to provide a robust solution for maintaining conversational context and user information across interactions. It leverages two different types of databases:

- **SQLite**: For structured data storage and retrieval of user information and persistent state
- **ChromaDB**: For vector-based storage enabling semantic search and contextual memory

## Features

- 🔄 Persistent user data storage with SQLite
- 🧠 Semantic memory capabilities using ChromaDB vector database
- 🔍 Case-insensitive user handling
- 🚫 Duplicate prevention for user entries
- ✅ Structured data validation and management
- 📄 PDF document processing and analysis
- 📚 Research lab collections management
- 📖 Google Scholar paper tracking and crawling
- 🔬 Paper recommendation based on research interests
- 📝 Contextual paper summarization with related research


### Research Lab Management

- Create lab collections with persistent information (name, institution, leader, members, etc.)
- Add lab members with their Google Scholar profiles
- Track papers published by lab members

### Paper Collection and Recommendation

- Automatically crawl Google Scholar pages of lab members to collect their arXiv papers
- Check for new papers by lab members during conversations
- Store PDF documents in the data directory with proper organization
- Recommend relevant papers from arXiv based on the lab's research interests and time period
- Save recommended papers and their embeddings to prevent duplication

### Paper Summarization

- Generate comprehensive paper summaries for specific lab member papers
- Utilize complete paper content for more thorough and accurate summaries of target papers
- Extract semantic sections (introduction, conclusion) from LaTeX source files of related papers when available
- Include contextual information from related papers in the lab collection
- Draw insights from both lab papers and recommended papers
- Provide academic-style summaries with key findings, methodologies, and relationships to existing research

## LinkedIn Integration

### Setting Up LinkedIn API Access

1. Create a LinkedIn Developer Application:
   - Go to [LinkedIn Developer Portal](https://developer.linkedin.com/)
   - Create a new app providing:
     - App Name
     - Application logo
     - LinkedIn company page (required, cannot be a profile page)
   - Accept terms and conditions

2. Configure Application Permissions:
   - Under Products, request access for:
     - "Share on LinkedIn" (adds w_member_social scope)
     - "Sign In with LinkedIn using OpenID Connect" (adds openid and email scopes)

3. Generate Access Token:
   For personal use (recommended):
   - Go to [LinkedIn OAuth2 tools](https://www.linkedin.com/developers/tools/oauth)
   - Generate a token with scopes: `w_member_social openid email profile'
   - Note: Access tokens are valid for 60 days

4. Get User ID:
   ```bash
   curl --location 'https://api.linkedin.com/v2/userinfo' \
   --header 'Authorization: Bearer YOUR_ACCESS_TOKEN'
   ```
   Save the returned user ID.

### Environment Configuration

Add the following to your `.env` file:

```env
LINKEDIN_USER_ID=your_user_id
LINKEDIN_ACCESS_TOKEN=your_access_token
```

#### Features:
- Supports text posts with formatting and emojis
- Configurable visibility (PUBLIC or CONNECTIONS)
- Automatic error handling and validation
- Environment-based configuration

#### Agent Integration:
The LinkedIn publisher is available as an agent tool and can be used with the following parameters:
- `commentary`: The content of the post
- `visibility`: Post visibility setting ("PUBLIC" or "CONNECTIONS")

### Important Notes:
- LinkedIn access tokens expire after 60 days

## Project Structure

```
branding_king/
├── branding_king/           # Main package directory
│   ├── tools/               # Tool implementations
│   │   ├── sqlite.py        # Entity database operations
│   │   ├── chromadb.py      # Vector database operations
│   │   ├── paper_crawler.py # Paper collection and recommendation tools
│   │   └── linkedin_publisher.py # LinkedIn posting automation
│   ├── agent.py             # Core agent implementation
│   ├── data/                # Data storage directory
│   │   ├── <lab_name>/      # Lab-specific paper PDFs
│   │   └── recommendation/  # Recommended paper PDFs
│   ├── .env                 # Environment configuration
│   └── .secrets.toml        # Secret configuration (not tracked)
└── frontend/                # Frontend implementation
```

## Prerequisites

- Python 3.11 or higher
- OpenAI API key (gpt-4o model for paper summarization and embeddings)
- Internet connection for accessing Google Scholar and arXiv
- Conda package manager

## Installation

1. Clone the repository:

```bash
git clone https://github.com/BenNotBen1224/Branding-King.git
cd Branding-King
```

2. Create and activate a conda environment:

```bash
cd branding_king
conda create -n branding-king python=3.11
conda activate branding-king
conda install -c conda-forge chromadb openai langchain beautifulsoup4 selenium
pip install -e .
```

## Configuration

1. Create necessary configuration files in the `branding_king` directory:

### .env

```env
# OpenAI Configuration
OPENAI_API_KEY_AGENT=your_agent_api_key
OPENAI_EMBEDDING_MODEL=text-embedding-3-large
OPENAI_API_KEY_SUMMARIZER=your_summarizer_api_key

# Database Paths
CHROMA_PERSIST_DIRECTORY=./chroma_langchain_db
SQLITE_DB_PATH=./sqlite_langchain_db.db
DEFAULT_DATA_DIR=./data

# LinkedIn Configuration
LINKEDIN_USER_ID=your_user_id
LINKEDIN_ACCESS_TOKEN=your_access_token
```

## Usage


1. Run the agent:

```bash
cd branding_king
python agent.py
```

2. Example interactions:

```
# Create a new research lab
> Create a lab called vision_research_lab at University of California, Berkeley, with leader Jitendra Malik

# Add members with their Google Scholar profiles
> Add member Haozhi Qi with scholar URL https://scholar.google.com/citations?user=iyVHKkcAAAAJ&hl=en to vision_research_lab

# Add research areas for the lab
> Add computer vision, machine learning and robotics as research areas for vision_research_lab

# Add website and description for the lab
> Add https://people.eecs.berkeley.edu/~malik/ as the website for vision_research_lab, and add description for the lab: Vision Intelligence

# Crawl Google Scholar for papers by lab members
> Collect papers from vision_research_lab members

# Stay updated with the lab
> Check new papers for vision_research_lab

# Get paper recommendations
> Recommend 5 papers from the last 30 days related to vision_research_lab

# Generate a paper summary
> Summarize the latest paper by Haozhi Qi from vision_research_lab

# Share paper summary on LinkedIn
> Share the paper summary on LinkedIn publicly with appropriate hashtags
```

## Development

- Run tests: `pytest`
- Format code: `black . && isort .`
- Type checking: `mypy .`

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [LangChain](https://github.com/langchain-ai/langchain) for the core agent capabilities
- [ChromaDB](https://github.com/chroma-core/chroma) for vector storage
- [OpenAI](https://openai.com/) for embedding and completion APIs
- [arXiv](https://arxiv.org/) for access to research papers
- [Zotero-arXiv-Daily](https://github.com/TideDra/zotero-arxiv-daily) for inspiration on paper recommendation
- [Trae IDE](https://trae.tech/) for providing an excellent development environment