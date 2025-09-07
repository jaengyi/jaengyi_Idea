# Ebook Classification and Summarization Program Plan
# Ebook 분류 및 요약 프로그램 기획

## 1. Objective
The primary goal of this project is to develop an automated program that scans a user's local directory of ebooks. For each ebook, the program will identify its content, classify it by genre or keywords, and then extract its preface or generate a concise summary. This will create a searchable and organized catalog of the user's ebook collection.

### 한글 번역
이 프로젝트의 주요 목표는 사용자의 로컬 ebook 디렉토리를 스캔하는 자동화된 프로그램을 개발하는 것입니다. 각 ebook에 대해 프로그램은 내용을 식별하고, 장르나 키워드로 분류한 다음, 서문을 추출하거나 간결한 요약을 생성합니다. 이를 통해 사용자의 ebook 컬렉션에 대한 검색 가능하고 체계적인 카탈로그를 만들 수 있습니다.

## 2. Key Features
- **Automated Directory Scanning:** The program will automatically detect ebook files (e.g., EPUB, PDF, TXT) within a specified folder and its subfolders.
- **Multi-Format Support:** It will be capable of parsing and extracting text from various common ebook formats.
- **Content Analysis:**
    - **Preface Extraction:** Intelligently identify and extract the book's preface, introduction, or foreword.
    - **AI-Powered Summarization:** If a preface is not available or a summary is preferred, use a Large Language Model (LLM) to generate a brief plot summary.
- **Automatic Classification:** Analyze the book's content to automatically assign relevant genres (e.g., Fiction, Sci-Fi, History, Technology) and keywords.
- **Structured Data Output:** Save the collected information (File Path, Title, Author, Genre, Keywords, Summary/Preface) into a structured format like a CSV file, JSON, or a simple SQLite database for easy access and management.

### 한글 번역
- **자동 디렉토리 스캔:** 프로그램은 지정된 폴더와 하위 폴더 내에서 ebook 파일(예: EPUB, PDF, TXT)을 자동으로 감지합니다.
- **다중 포맷 지원:** 다양한 일반 ebook 형식에서 텍스트를 파싱하고 추출할 수 있습니다.
- **콘텐츠 분석:**
    - **서문 추출:** 책의 서문, 소개 또는 머리말을 지능적으로 식별하고 추출합니다.
    - **AI 기반 요약:** 서문을 사용할 수 없거나 요약이 선호되는 경우, 대규모 언어 모델(LLM)을 사용하여 간략한 줄거리 요약을 생성합니다.
- **자동 분류:** 책의 내용을 분석하여 관련 장르(예: 소설, 공상 과학, 역사, 기술)와 키워드를 자동으로 할당합니다.
- **구조화된 데이터 출력:** 수집된 정보(파일 경로, 제목, 저자, 장르, 키워드, 요약/서문)를 CSV 파일, JSON 또는 간단한 SQLite 데이터베이스와 같은 구조화된 형식으로 저장하여 쉽게 접근하고 관리할 수 있도록 합니다.

## 3. Detailed Methodology (Step-by-Step)

**Step 1: File Discovery**
- The user provides a path to their ebook library.
- The program recursively scans the directory to find all files with supported extensions (e.g., `.epub`, `.pdf`, `.txt`).

**Step 2: Text Extraction**
- For each file, the program determines its format.
- It uses a specific library to open the ebook and extract its raw text content.
    - For **EPUB:** Use a library to parse the file and extract text from its chapters, ignoring HTML tags.
    - For **PDF:** Use a library to extract text. This can be challenging for multi-column layouts or image-based PDFs.
    - For **TXT:** Read the text directly.
- The program will also attempt to extract metadata like Title and Author if available within the ebook file.

**Step 3: Content Processing (Preface or Summary)**
- The extracted text is processed to find the preface. The program can search for headings like "Preface," "Introduction," "Foreword," or "Prologue" and extract the subsequent text until the first chapter begins.
- If a preface cannot be reliably found, the program will take the first 1,000-2,000 words of the book and send them to an external LLM API (like Google Gemini or OpenAI GPT).
- The API request will be a prompt such as: `"Based on the following text from a book, please provide a concise one-paragraph summary."`

**Step 4: Classification**
- The same extracted text or the generated summary will be sent to the LLM API with a different prompt.
- The prompt will be something like: `"Analyze the following book summary and provide up to 5 relevant genres and 10 keywords. Format the output as JSON with 'genres' and 'keywords' keys."`
- The program will parse the structured output from the API to get the classification data.

**Step 5: Data Storage**
- All the gathered information (file path, title, author, summary, genres, keywords) is compiled into a single record.
- This record is then appended to the chosen output file (e.g., a new row in a CSV file or a new entry in a database).
- The process repeats for all discovered ebook files.

### 한글 번역

**1단계: 파일 탐색**
- 사용자는 자신의 ebook 라이브러리 경로를 제공합니다.
- 프로그램은 지원되는 확장자(예: .epub, .pdf, .txt)를 가진 모든 파일을 찾기 위해 디렉토리를 재귀적으로 스캔합니다.

**2단계: 텍스트 추출**
- 각 파일에 대해 프로그램은 형식을 결정합니다.
- 특정 라이브러리를 사용하여 ebook을 열고 원시 텍스트 콘텐츠를 추출합니다.
    - **EPUB의 경우:** 라이브러리를 사용하여 파일을 파싱하고 HTML 태그를 무시하면서 챕터에서 텍스트를 추출합니다.
    - **PDF의 경우:** 라이브러리를 사용하여 텍스트를 추출합니다. 다중 열 레이아웃이나 이미지 기반 PDF의 경우 어려울 수 있습니다.
    - **TXT의 경우:** 텍스트를 직접 읽습니다.
- 프로그램은 또한 ebook 파일 내에서 사용 가능한 경우 제목 및 저자와 같은 메타데이터를 추출하려고 시도합니다.

**3단계: 콘텐츠 처리 (서문 또는 요약)**
- 추출된 텍스트는 서문을 찾기 위해 처리됩니다. 프로그램은 "Preface," "Introduction," "Foreword," 또는 "Prologue"와 같은 제목을 검색하고 첫 번째 챕터가 시작될 때까지 후속 텍스트를 추출할 수 있습니다.
- 서문을 안정적으로 찾을 수 없는 경우, 프로그램은 책의 처음 1,000-2,000 단어를 가져와 외부 LLM API(예: Google Gemini 또는 OpenAI GPT)로 보냅니다.
- API 요청은 다음과 같은 프롬프트가 될 것입니다: `"Based on the following text from a book, please provide a concise one-paragraph summary."`

**4단계: 분류**
- 동일한 추출된 텍스트 또는 생성된 요약이 다른 프롬프트와 함께 LLM API로 전송됩니다.
- 프롬프트는 다음과 같을 것입니다: `"Analyze the following book summary and provide up to 5 relevant genres and 10 keywords. Format the output as JSON with 'genres' and 'keywords' keys."`
- 프로그램은 API의 구조화된 출력을 파싱하여 분류 데이터를 얻습니다.

**5단계: 데이터 저장**
- 수집된 모든 정보(파일 경로, 제목, 저자, 요약, 장르, 키워드)는 단일 레코드로 컴파일됩니다.
- 이 레코드는 선택한 출력 파일(예: CSV 파일의 새 행 또는 데이터베이스의 새 항목)에 추가됩니다.
- 이 과정은 발견된 모든 ebook 파일에 대해 반복됩니다.

## 4. Proposed Technology Stack
- **Programming Language:** **Python 3** - Excellent for text processing, file handling, and has a rich ecosystem of libraries.
- **Ebook Parsing Libraries:**
    - **`EbookLib`:** For parsing EPUB files.
    - **`PyMuPDF (fitz)`:** For robustly extracting text from PDF files.
- **AI & API Interaction:**
    - **`google-generativeai`** or **`openai`:** Python clients for interacting with LLM APIs for summarization and classification.
    - **`requests`:** For general HTTP requests.
- **Data Output:**
    - **`csv`:** Standard Python library for writing to CSV files.
    - **`json`:** Standard Python library for handling JSON data.
    - **`sqlite3`:** Standard Python library for a lightweight, file-based database.

### 한글 번역
- **프로그래밍 언어:** **Python 3** - 텍스트 처리, 파일 처리에 탁월하며 풍부한 라이브러리 생태계를 가지고 있습니다.
- **Ebook 파싱 라이브러리:**
    - **`EbookLib`:** EPUB 파일 파싱용.
    - **`PyMuPDF (fitz)`:** PDF 파일에서 텍스트를 안정적으로 추출하기 위함.
- **AI 및 API 상호 작용:**
    - **`google-generativeai`** 또는 **`openai`:** 요약 및 분류를 위해 LLM API와 상호 작용하는 Python 클라이언트.
    - **`requests`:** 일반 HTTP 요청용.
- **데이터 출력:**
    - **`csv`:** CSV 파일에 쓰기 위한 표준 Python 라이브러리.
    - **`json`:** JSON 데이터를 처리하기 위한 표준 Python 라이브러리.
    - **`sqlite3`:** 경량 파일 기반 데이터베이스를 위한 표준 Python 라이브러리.

## 5. Considerations & Potential Challenges
- **DRM (Digital Rights Management):** Many commercially purchased ebooks are protected by DRM, which will prevent the program from reading their content. This program will only work for DRM-free ebooks.
- **PDF Layout Complexity:** Text extraction from PDFs with complex layouts (multiple columns, tables, embedded images) can be inaccurate.
- **API Costs & Rate Limits:** The use of external LLM APIs will incur costs based on the amount of text processed. It's important to implement the logic efficiently to minimize API calls. Rate limiting on the API side should also be handled.
- **Accuracy:** The quality of the summaries and classifications will depend entirely on the capabilities of the chosen LLM and the quality of the prompts.
- **Error Handling:** The program must be robust enough to handle corrupted files, unsupported formats, or network errors when calling APIs, allowing it to skip problematic files and continue the process.

### 한글 번역
- **DRM (디지털 저작권 관리):** 많은 상업적으로 구매한 ebook은 DRM으로 보호되어 프로그램이 내용을 읽지 못하게 합니다. 이 프로그램은 DRM이 없는 ebook에서만 작동합니다.
- **PDF 레이아웃 복잡성:** 복잡한 레이아웃(다중 열, 표, 내장 이미지)을 가진 PDF에서 텍스트를 추출하는 것은 부정확할 수 있습니다.
- **API 비용 및 속도 제한:** 외부 LLM API를 사용하면 처리된 텍스트 양에 따라 비용이 발생합니다. API 호출을 최소화하기 위해 로직을 효율적으로 구현하는 것이 중요합니다. API 측의 속도 제한도 처리해야 합니다.
- **정확성:** 요약 및 분류의 품질은 선택한 LLM의 기능과 프롬프트의 품질에 전적으로 달려 있습니다.
- **오류 처리:** 프로그램은 손상된 파일, 지원되지 않는 형식 또는 API 호출 시 네트워크 오류를 처리할 수 있을 만큼 견고해야 하며, 문제가 있는 파일을 건너뛰고 프로세스를 계속할 수 있어야 합니다.