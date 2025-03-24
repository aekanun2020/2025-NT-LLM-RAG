# 2025-NT-Understanding LLM and Implementing RAG

## Hands-on 2: การเปรียบเทียบ Node Parsers และการสร้าง Embedding

## รายละเอียด
Hands-on นี้เน้นเรื่องการเปรียบเทียบระหว่าง Node Parsers ประเภทต่างๆ (MarkdownNodeParser และ SentenceSplitter) ในการแบ่ง nodes จากเอกสาร Markdown และการสร้าง Embedding สำหรับระบบ RAG โดยใช้ LlamaIndex

## จุดประสงค์การเรียนรู้
* เข้าใจความแตกต่างและข้อดีข้อเสียระหว่าง MarkdownNodeParser และ SentenceSplitter ในการแบ่งเอกสาร
* วิเคราะห์ผลลัพธ์จากการใช้ parser ทั้ง 2 แบบ
* เรียนรู้วิธีการสร้าง Embedding จาก nodes ที่แบ่งด้วยวิธีต่างๆ
* เข้าใจคุณสมบัติของ Embedding ที่ไม่ว่าข้อความจะสั้นหรือยาวก็มีมิติเท่ากัน

## ขั้นตอนการทำงาน
1. ติดตั้ง LlamaIndex และ dependencies ที่จำเป็น
2. ดาวน์โหลด corpus เอกสาร Markdown
3. โหลดเอกสารโดยใช้ SimpleDirectoryReader
4. เปรียบเทียบผลลัพธ์ระหว่าง MarkdownNodeParser และ SentenceSplitter
   - จำนวน nodes ที่แตกต่างกัน
   - โครงสร้าง metadata ที่เก็บไว้
   - วิธีการแบ่งเนื้อหาที่มีหัวข้อชัดเจน
5. สร้าง Embedding จาก nodes ที่แบ่งด้วยแต่ละวิธี
6. วิเคราะห์ขนาดมิติของ Embedding เพื่อแสดงให้เห็นว่าไม่ว่าข้อความจะยาวเท่าไรก็ได้ embedding ที่มีขนาดเท่ากัน

## คำอธิบายโค้ด
- **การตั้งค่าและการดาวน์โหลด corpus**: ติดตั้ง libraries และดาวน์โหลดเอกสาร Markdown
- **ส่วนที่ 1**: การเปรียบเทียบระหว่าง MarkdownNodeParser และ SentenceSplitter แสดงความแตกต่างในการแบ่ง nodes
- **ส่วนที่ 2**: แสดงขั้นตอนการ Splitting จนถึงการสร้าง Embedding โดยใช้ HuggingFace Embedding (all-MiniLM-L6-v2)

## ความแตกต่างสำคัญระหว่าง Parsers
- **MarkdownNodeParser**: แบ่งตามโครงสร้าง Markdown (หัวข้อ) เก็บข้อมูล header_path ไว้
- **SentenceSplitter**: แบ่งตามขนาดที่กำหนด (chunk_size) รักษาขอบเขตประโยค ไม่สนใจโครงสร้าง Markdown

## คุณสมบัติพิเศษของ Embedding
- ไม่ว่าข้อความจะสั้นหรือยาวเพียงใด vector ที่ได้จาก embedding model จะมีมิติเท่ากันเสมอ
- สำหรับ model all-MiniLM-L6-v2 จะสร้าง vector ขนาด 384 มิติ
- คุณสมบัตินี้ทำให้สามารถคำนวณความคล้ายระหว่างข้อความที่มีความยาวต่างกันได้

## การใช้งาน

### ทางเลือกในการรันโค้ด

1. **Google Colab (แนะนำ)**
   * เข้าถึงโค้ดได้ที่: [https://colab.research.google.com/drive/1FItIdXnoYMKlljmENSF1dCxF6NIJRdum?usp=sharing](https://colab.research.google.com/drive/1FItIdXnoYMKlljmENSF1dCxF6NIJRdum?usp=sharing)
   * สามารถรันได้ทันทีโดยไม่ต้องติดตั้งอะไรเพิ่มเติม
   * แนะนำให้รันโค้ดแบบต่อเนื่องทีละเซลล์เพื่อดูผลลัพธ์ทุกขั้นตอน

2. **Local Notebook ด้วย Conda (แนะนำสำหรับการรันในเครื่อง)**
   * สำหรับทั้งผู้ใช้ **Windows** และ **Mac M-series**:
     - ติดตั้ง Miniconda หรือ Anaconda: [ดาวน์โหลด Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html)
     - สร้าง Conda environment ด้วย Python 3.8.18:
       ```bash
       conda create -n advrag python=3.8.18
       conda activate advrag
       ```
     - ติดตั้ง dependencies:
       ```bash
       pip install llama-index llama-index-embeddings-huggingface jupyter
       ```
     - ตรวจสอบว่าใช้ Python 3.8.18 จริง:
       ```bash
       python --version
       ```
     - ดาวน์โหลดไฟล์ notebook และรันด้วย:
       ```bash
       jupyter notebook
       ```
     
   * คำแนะนำเพิ่มเติมสำหรับผู้ใช้ **Mac M-series**:
     - Conda จะจัดการ compatibility สำหรับ Apple Silicon โดยอัตโนมัติ
     - หากยังมีปัญหากับ libraries บางตัว ให้ติดตั้งเวอร์ชันที่เฉพาะเจาะจงสำหรับ ARM64:
       ```bash
       conda install -c conda-forge package-name
       ```

เมื่อรันโค้ดในแต่ละส่วน ให้สังเกตผลลัพธ์ที่แสดงความแตกต่างระหว่าง parser ทั้งสองประเภท และวิเคราะห์ว่ากรณีใดเหมาะกับการใช้งานแบบใด รวมถึงสังเกตคุณสมบัติของ embedding ที่มีขนาดมิติเท่ากันไม่ว่าข้อความจะสั้นหรือยาวเพียงใด