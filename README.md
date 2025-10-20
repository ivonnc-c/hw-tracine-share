# hw-tracine-share
作業追蹤網站分享版
建站流程:
前置作業
1.	註冊 GitHub 帳號： 前往 GitHub 網站，註冊一個免費帳號。
2.	註冊 Vercel 帳號： 前往 Vercel 網站，選擇用你的 GitHub 帳號註冊，這是最快的方式。
第一步：建立自己的 Firebase 資料庫 (後端)
1.	建立 Firebase 專案：
o	前往 Firebase 控制台。
o	點擊「新增專案」，幫專案取一個你喜歡的名字（例如：my-homework-tracker）。
o	下一步，可以關閉「為這個專案啟用 Google Analytics (分析)」，然後建立專案。
2.	建立 Firestore 資料庫：
o	在專案頁面左邊的選單，點擊「Build」>「Firestore Database」。
o	點擊「建立資料庫」。
o	選擇「以正式版模式啟動」（這很重要，安全性較高），然後點「下一步」。
o	選擇離你最近的地區（例如 asia-east1 (台灣)），然後點「啟用」。
3.	設定安全規則 (最關鍵的一步！)：
o	在 Firestore Database 頁面，點擊上方的「規則」分頁。
o	把編輯器裡所有的預設文字都刪掉。
o	完整複製並貼上以下所有規則：
JavaScript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    match /artifacts/{appId}/public/data/{collection}/{docId} {
      allow read, write: if request.auth != null && request.auth.uid == resource.data.userId;
      allow create: if request.auth != null && request.auth.uid == request.resource.data.userId;
    }

    match /artifacts/{appId}/publicData/{classId} {
      allow read: if true;
      allow write: if request.auth != null 
                   && get(/databases/$(database)/documents/artifacts/$(appId)/public/data/classes/$(classId)).data.userId == request.auth.uid;
    }
  }
}
o	點擊「發布」。
4.	開啟 Google 登入功能：
o	在左邊選單，點擊「Build」>「Authentication」。
o	點擊「開始使用」。
o	在登入服務供應商列表中，選擇「Google」並將其「啟用」，然後儲存。
5.	取得你的 Firebase 設定檔 (Config)：
o	點擊左上角專案名稱旁邊的「齒輪」圖示，進入「專案設定」。
o	在「一般」分頁下方，找到「你的應用程式」區塊。
o	點擊網頁圖示 </> 來註冊一個新的網頁應用程式。
o	幫應用程式取個名字，然後點「註冊應用程式」。
o	下一步，你會看到一個 firebaseConfig 的程式碼區塊，這就是你的金鑰！ 等一下會用到。
第二步：用 Vercel 建立自己的網站 (前端)
1.	複製程式碼藍圖 (Fork)：
o	前往家禎老師的 GitHub 專案頁面 (你需要提供這個連結給他們)。
o	在頁面右上角，找到並點擊「Fork」按鈕。這會把整個專案完整地複製一份到你自己的 GitHub 帳號底下。
2.	在 Vercel 建立新專案：
o	登入 Vercel 儀表板。
o	點擊「Add New...」>「Project」。
o	在「Import Git Repository」區塊，Vercel 應該會自動找到你剛剛 Fork 的那個專案。點擊它旁邊的「Import」。
o	你不需要做任何設定，直接點擊「Deploy」即可。
3.	完成！
o	等待幾秒鐘，Vercel 就會幫你把網站建立好。
o	點擊 Vercel 提供的網址，打開你的網站。第一次開啟時，會看到「首次使用設定」頁面。
o	回到你剛剛在 Firebase 複製的 firebaseConfig，將裡面的 apiKey, authDomain 等資訊，逐一複製貼上到網站對應的欄位中，然後儲存。
