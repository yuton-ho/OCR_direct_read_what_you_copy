# OCR_direct_read_what_you_copy
If you copy a image, use this program it can read what you copy by OCR.
```
開始
  ↓
嘗試使用模組PIL的grabclipboard抓取剪貼簿
  ↓
成功抓取，如果失敗或複製的不是文字出現"剪貼板中沒有圖像。
  ↓
使用ocr_scan讀取
  ↓
執行辨識
  ↓
顯示辨識結果
  ↓
結束
```
我選擇辨識的是英文，想客製辨識在reader.readtext()中修改。
這次的修改聚焦在ocr_scan()，加入
    reader = easyocr.Reader(['en'], gpu=False)#easyocr.Reader(語言, gpu=是否需要)
    
    # 根據輸入圖像的不同類型進行適當的轉換處理
    if isinstance(image, str):
        # 如果輸入是字符串，則假定它是圖像文件的路徑，直接傳給easyocr
        img_for_ocr = image
    elif isinstance(image, np.ndarray):
        # 如果輸入是numpy數組，直接使用
        img_for_ocr = image
    elif isinstance(image, Image.Image):
        # 如果輸入是PIL圖像對象，轉換為numpy數組
        img_for_ocr = np.array(image)
    elif isinstance(image, bytes):
        # 如果輸入是二進制數據，先轉為PIL圖像再轉為numpy數組
        img_for_ocr = np.array(Image.open(io.BytesIO(image)))
    else:
        # 如果輸入類型不是上述任何一種，則拋出類型錯誤
        raise TypeError("不支援的圖片類型。請提供PIL Image、numpy數組、bytes或檔案路徑。")
