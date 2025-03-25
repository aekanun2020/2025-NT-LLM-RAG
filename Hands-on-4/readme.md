# 2025-NT-Understanding LLM and Implementing RAG

## Hands-on 4: การค้นหาข้อมูลโดยใช้ Retriever กับ OpenSearch

## รายละเอียด
ต่อเนื่องจาก Hands-on 3 ที่ได้สร้าง Vector Indexes และบันทึกลงใน OpenSearch Vector Database แล้ว Hands-on 4 นี้จะเน้นการนำ Vector Indexes ที่บันทึกไว้ใน OpenSearch มาใช้ในการค้นหาข้อมูลแบบ Hybrid Search (ผสมผสานระหว่าง Vector Search และ Keyword Search) โดยใช้ search pipeline ของ OpenSearch ร่วมกับ Retriever จาก LlamaIndex เพื่อค้นหาข้อมูลสุขภาพเกี่ยวกับโรคหัดและโรคหัดเยอรมัน

## จุดประสงค์การเรียนรู้
* เข้าใจวิธีการใช้ Retriever เพื่อค้นหาข้อมูลจาก Vector Indexes
* เรียนรู้การใช้งาน Hybrid Search ใน OpenSearch
* ฝึกการสร้างฟังก์ชันค้นหาและแสดงผลแบบไม่ตัดทอนข้อมูล
* ทดลองตั้งคำถามและดูผลลัพธ์การค้นหา

## ขั้นตอนการทำงาน
1. ติดตั้ง LlamaIndex และ dependencies ที่จำเป็น
2. ตั้งค่าเชื่อมต่อกับ OpenSearch
3. ตั้งค่า Embedding Model (BAAI/bge-m3)
4. สร้าง Vector Store และเชื่อมต่อกับ OpenSearch
5. สร้าง Vector Index จาก Vector Store ที่มีอยู่แล้ว
6. สร้างฟังก์ชันค้นหาและแสดงผลแบบไม่ตัดทอนข้อมูล
7. ทดสอบการค้นหาด้วยคำถามที่กำหนดไว้ล่วงหน้า
8. อนุญาตให้ผู้ใช้ป้อนคำถามเพิ่มเติม

## คำอธิบายโค้ด
- **การติดตั้ง**: ติดตั้งไลบรารีที่จำเป็น เช่น llama-index, llama-index-embeddings-huggingface, llama-index-vector-stores-opensearch
- **การเชื่อมต่อ OpenSearch**: กำหนดค่า endpoint, index name และฟิลด์ที่ใช้ในการค้นหา โดยเชื่อมต่อกับ Vector Index ที่บันทึกไว้แล้วใน Hands-on 3
- **การตั้งค่า Embedding Model**: ใช้ HuggingFaceEmbedding กับโมเดล BAAI/bge-m3 เพื่อ embedding คำถามใหม่ให้อยู่ในรูปแบบเดียวกับที่บันทึกไว้
- **การสร้าง Vector Store**: ใช้ OpensearchVectorClient และ OpensearchVectorStore เพื่อเชื่อมต่อกับ OpenSearch โดยระบุ search_pipeline เป็น "hybrid-search-pipeline" เพื่อใช้ Hybrid Search
- **การสร้าง Vector Index**: สร้าง VectorStoreIndex จาก Vector Store ที่มีอยู่แล้วใน OpenSearch โดยไม่ต้องอัปโหลดข้อมูลใหม่
- **ฟังก์ชันค้นหา**: สร้างฟังก์ชัน search_without_truncation ที่ใช้ Retriever ในโหมด HYBRID (ใช้ search pipeline ของ OpenSearch) และไม่ตัดทอนผลลัพธ์
- **ฟังก์ชันแสดงผล**: สร้างฟังก์ชัน display_search_results สำหรับแสดงผลลัพธ์การค้นหาในรูปแบบที่อ่านง่าย พร้อมทั้งแสดง metadata และคะแนนความเกี่ยวข้อง

## คำถามที่กำหนดไว้ล่วงหน้า
ระบบมีคำถามที่กำหนดไว้ล่วงหน้าเพื่อทดสอบการค้นหา ได้แก่:
1. โรคหัดและโรคหัดเยอรมันแตกต่างกันอย่างไร? (คำถามเปรียบเทียบ)
2. อธิบายสาเหตุของโรคหัดเยอรมันและการป้องกัน (คำถามหลายประเด็น)
3. ทำไมโรคหัดเยอรมันจึงมีอันตรายกับหญิงตั้งครรภ์? (คำถามวิเคราะห์เชิงลึก)
4. ถ้าคนที่ฉีดวัคซีนป้องกันโรคหัดเยอรมันแล้ว จะมีโอกาสติดเชื้อหรือไม่? (คำถามสมมติเหตุการณ์)
5. โรคหัดเยอรมันมีผลกระทบอย่างไรต่อระบบสาธารณสุขและเศรษฐกิจของประเทศ? (คำถามข้ามสาขา)
6. การรักษาโรคหัดเยอรมันที่ดีที่สุดคืออะไร? (คำถามกำกวม)

## ความสำคัญของ Hybrid Search
- **ผสมผสานการค้นหา**: รวม semantic search (vector) กับ keyword search (text) โดยใช้ search pipeline ของ OpenSearch
- **เพิ่มประสิทธิภาพการค้นหา**: ช่วยให้ค้นพบข้อมูลที่เกี่ยวข้องได้ดียิ่งขึ้น
- **แสดงคะแนนความเกี่ยวข้อง**: ผลลัพธ์แสดงคะแนนที่คำนวณจากทั้ง semantic และ keyword matching
- **ใช้ประโยชน์จากข้อมูลที่บันทึกไว้**: เข้าถึงข้อมูล Vector Embeddings ที่บันทึกไว้ใน OpenSearch จาก Hands-on 3 โดยไม่ต้องสร้างใหม่

## ข้อควรระวัง
- **ชื่อ Index**: ต้องใช้ชื่อ index ที่ถูกต้องตามที่สร้างไว้ใน Hands-on 3 (ในตัวอย่างคือ "aekanun_doc_index" แต่ผู้ใช้ควรเปลี่ยนเป็นชื่อของตัวเอง)
- **การเชื่อมต่อ OpenSearch**: ต้องมีการเชื่อมต่ออินเทอร์เน็ตที่สามารถเข้าถึง OpenSearch endpoint ได้
- **การตั้งค่า search_pipeline**: ต้องแน่ใจว่ามีการตั้งค่า "hybrid-search-pipeline" ไว้แล้วใน OpenSearch (ซึ่งควรได้ดำเนินการไว้แล้วใน Hands-on 3)
- **Embedding Model**: โมเดล Embedding ที่ใช้ต้องเป็นตัวเดียวกับที่ใช้ใน Hands-on 3 (BAAI/bge-m3) เพื่อให้เวกเตอร์ของคำถามอยู่ในมิติและรูปแบบเดียวกันกับที่บันทึกไว้

## การใช้งาน

### ทางเลือกในการรันโค้ด

1. **Google Colab (แนะนำ)**
   * เข้าถึงโค้ดได้ที่: [https://colab.research.google.com/drive/1CyfI0C8omCvbmiAD_qy2EWk7Ecm7mIPI?usp=sharing](https://colab.research.google.com/drive/1CyfI0C8omCvbmiAD_qy2EWk7Ecm7mIPI?usp=sharing)
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

เมื่อรันโค้ดแล้ว ระบบจะเชื่อมต่อกับ OpenSearch เพื่อใช้ Vector Index ที่สร้างไว้แล้วใน Hands-on 3 (ที่บันทึกไว้ใน OpenSearch) ในการค้นหาข้อมูล โดยจะใช้ Hybrid Search Pipeline ของ OpenSearch ซึ่งผสมผสานการค้นหาแบบ Vector Search (Semantic) และ Keyword Search (Lexical) เข้าด้วยกัน ระบบจะแสดงผลลัพธ์ของคำถามที่กำหนดไว้ล่วงหน้า และอนุญาตให้ผู้ใช้ป้อนคำถามเพิ่มเติมได้

## ผลลัพธ์การค้นหา

ผลลัพธ์การค้นหาจะแสดงข้อมูลดังนี้:
1. **อันดับของผลลัพธ์** พร้อมคะแนนความเกี่ยวข้อง
2. **Metadata** ของผลลัพธ์ เช่น ชื่อไฟล์ต้นฉบับ, ตำแหน่งในเอกสาร
3. **เนื้อหาเต็ม** ของผลลัพธ์โดยไม่มีการตัดทอน

ผู้ใช้สามารถประเมินคุณภาพของผลลัพธ์และวิเคราะห์ว่าระบบ RAG สามารถตอบคำถามได้ตรงประเด็นหรือไม่ ซึ่งเป็นขั้นตอนสำคัญในการพัฒนาระบบ RAG ที่มีประสิทธิภาพ