# 2025-NT-Understanding LLM and Implementing RAG

## Hands-on 3: การสร้าง Vector Indexes และเชื่อมต่อกับ OpenSearch

## รายละเอียด
ต่อเนื่องจาก Hands-on 2 ที่เน้นการเปรียบเทียบ Node Parsers และการแบ่ง nodes จากเอกสาร Hands-on 3 นี้จะขยายความรู้ไปสู่การนำ nodes ไปใช้งานจริง โดยเน้นเรื่องการสร้าง Vector Indexes สำหรับระบบ RAG โดยใช้ LlamaIndex และการเชื่อมต่อกับ OpenSearch ซึ่งเป็น Vector Database ที่มีความสามารถในการทำ Hybrid Search เพื่อเพิ่มประสิทธิภาพในการค้นหาข้อมูล

## จุดประสงค์การเรียนรู้
* เข้าใจวิธีการสร้าง Vector Indexes ด้วย LlamaIndex
* เรียนรู้การเชื่อมต่อ LlamaIndex กับ OpenSearch
* เข้าใจการตั้งค่า Hybrid Search Pipeline ใน OpenSearch
* ฝึกการใช้งาน HuggingFace Embedding Model (BAAI/bge-m3)

## ขั้นตอนการทำงาน
1. ติดตั้ง LlamaIndex และ dependencies ที่จำเป็น
2. ดาวน์โหลด corpus เอกสาร Markdown
3. สร้าง Hybrid Search Pipeline สำหรับ OpenSearch
4. โหลดเอกสารโดยใช้ SimpleDirectoryReader
5. สร้าง nodes จากเอกสารโดยใช้ MarkdownNodeParser
6. ตั้งค่า Embedding Model (BAAI/bge-m3)
7. เชื่อมต่อกับ OpenSearch และสร้าง Vector Store
8. สร้าง Vector Indexes โดยบันทึกข้อมูลเข้าสู่ OpenSearch และบันทึกโครงสร้าง index ลงไฟล์ .pkl (เป็นเพียงตัวเลือกเสริม)

## คำอธิบายโค้ด
- **การติดตั้งและการดาวน์โหลด corpus**: ติดตั้ง libraries และดาวน์โหลดเอกสาร Markdown
- **การสร้าง Hybrid Search Pipeline**: ตั้งค่า pipeline สำหรับการทำ hybrid search ใน OpenSearch
- **การตั้งค่า Vector Store**: ตั้งค่า OpensearchVectorClient และสร้าง OpensearchVectorStore
- **การสร้าง Indices**: สร้าง VectorStoreIndex ซึ่งจะบันทึก vector embeddings ของเอกสารลงใน OpenSearch และบันทึกโครงสร้าง index ลงในไฟล์ .pkl (เป็นเพียงตัวเลือกเสริม)

## ความสำคัญของ Hybrid Search
- **ผสมผสานการค้นหา**: รวม semantic search (vector) กับ keyword search (text)
- **เพิ่มประสิทธิภาพการค้นหา**: ช่วยให้ค้นพบข้อมูลที่เกี่ยวข้องได้ดียิ่งขึ้น
- **ปรับแต่งน้ำหนัก**: สามารถปรับค่า weights ระหว่าง semantic และ keyword search ได้

## ข้อควรระวัง
- **ชื่อ Index**: ต้องเปลี่ยน `OPENSEARCH_INDEX` ให้เป็นชื่อของตัวเอง ในรูปแบบ `yourname_doc_index` (ตัวพิมพ์เล็กทั้งหมด)
- **การเชื่อมต่อ OpenSearch**: ต้องมีการเชื่อมต่ออินเทอร์เน็ตที่สามารถเข้าถึง OpenSearch endpoint ได้

## การใช้งาน

### ทางเลือกในการรันโค้ด

1. **Google Colab (แนะนำ)**
   * เข้าถึงโค้ดได้ที่: [https://colab.research.google.com/drive/1z939r-Y817lUqzOoR8lglcVvztWQRpnA?usp=sharing](https://colab.research.google.com/drive/1z939r-Y817lUqzOoR8lglcVvztWQRpnA?usp=sharing)
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
       pip install llama-index llama-index-embeddings-huggingface llama-index-vector-stores-opensearch requests nest_asyncio jupyter
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

เมื่อรันโค้ดแล้ว ระบบจะบันทึก vector embeddings ของเอกสารและ metadata ลงใน OpenSearch database ซึ่งเป็น vector database หลักสำหรับระบบ RAG ของเรา

นอกจากนี้ โค้ดยังบันทึกโครงสร้าง index ลงในไฟล์ .pkl ในเครื่องของคุณ แต่ไฟล์นี้เป็นเพียงส่วนเสริมเท่านั้น มีประโยชน์ในกรณีต่อไปนี้:
- เมื่อต้องการทดสอบระบบแบบ offline โดยไม่ต้องเชื่อมต่อ OpenSearch
- เพื่อเป็นการสำรองข้อมูลโครงสร้าง index หากต้องการย้ายไปยังระบบอื่น
- เพื่อความเร็วในการโหลด index โดยไม่ต้องสร้าง connection ใหม่กับ OpenSearch

ในการใช้งานจริง หากเรามีการเชื่อมต่อกับ OpenSearch อยู่เสมอ เราสามารถใช้ข้อมูลจาก OpenSearch โดยตรงได้เลย โดยไม่จำเป็นต้องใช้ไฟล์ .pkl แต่อย่างใด

## การตรวจสอบผลลัพธ์

หลังจากที่คุณได้สร้าง index และบันทึกข้อมูลลงใน OpenSearch แล้ว คุณสามารถตรวจสอบผลลัพธ์ด้วยคำสั่ง curl ต่อไปนี้:

```bash
curl -X GET "http://34.41.37.53:9200/yourname_doc_index/_search?pretty" -H 'Content-Type: application/json' -d' {
  "query": {
    "match_all": {}
  }
} ' | more
```

อย่าลืมแทนที่ `yourname_doc_index` ด้วยชื่อ index ที่คุณใช้