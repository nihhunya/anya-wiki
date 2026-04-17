# Attention Residuals (AttnRes) 🧠

**แหล่งที่มา:** [[Attention-Residuals-Meta|Metadata]] | [arXiv:2603.15031](https://arxiv.org/abs/2603.15031v1)
**ผู้สร้าง:** Kimi Team
**แท็ก:** #LLM #Architecture #Attention #ResidualConnections #DeepLearning

## 📌 สรุปใจความสำคัญ
งานวิจัยนี้เสนอแนวคิด **Attention Residuals (AttnRes)** เพื่อแก้ปัญหาการสะสมของ Hidden-state ในโมเดล LLM ขนาดใหญ่ที่ใช้ PreNorm ซึ่งปกติจะใช้การบวกแบบถ่วงน้ำหนักคงที่ (Fixed Unit Weights) ทำให้เกิดปัญหา "PreNorm Dilution" (การเจือจางของเลเยอร์ลึกๆ)

### 🛠️ กลไกการทำงาน
- **จาก Fixed $\rightarrow$ Dynamic:** เปลี่ยนจากการบวกเลเยอร์ก่อนหน้าแบบปกติ เป็นการใช้ **Softmax Attention** เหนือผลลัพธ์ของเลเยอร์ที่ผ่านมาทั้งหมด
- **Input-Dependent Weights:** ทำให้แต่ละเลเยอร์สามารถ "เลือก" ได้ว่าจะนำข้อมูลจากเลเยอร์ไหนมาใช้มากน้อยเพียงใด ตามบริบทของ Input นั้นๆ
- **Block AttnRes:** เพื่อลดภาระด้าน Memory และการสื่อสาร (Communication Overhead) ในโมเดลขนาดใหญ่ จึงมีการแบ่งเลเยอร์เป็น "บล็อก" และทำการ Attention ในระดับบล็อกแทน ซึ่งยังคงประสิทธิภาพใกล้เคียงกับ Full AttnRes

## 📈 ผลลัพธ์และประโยชน์
- **Uniformity:** ช่วยให้ Magnitude ของ Output และการกระจายของ Gradient ในแต่ละระดับความลึกมีความสม่ำเสมอมากขึ้น
- **Performance:** ปรับปรุงประสิทธิภาพในงาน Downstream tasks ทั้งหมดเมื่อรวมเข้ากับสถาปัตยกรรม Kimi Linear (48B total / 3B activated parameters)
- **Drop-in Replacement:** สามารถนำไปใช้แทน Residual Connections แบบมาตรฐานได้ทันทีโดยมี Overhead ต่ำมาก

## 🔗 การเชื่อมโยงที่เกี่ยวข้อง
- ดูเพิ่มเติมเกี่ยวกับสถาปัตยกรรม LLM: [[LLM-Architecture-Overview]]
- เกี่ยวกับปัญหาของ Deep Networks: [[Vanishing-Gradient-Problem]]
