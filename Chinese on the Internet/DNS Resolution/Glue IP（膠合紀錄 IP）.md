**Glue IP（膠合紀錄 IP）** 通常是指==網域名稱系統（DNS）中，針對位於自身網域底下的自訂名稱伺服器（Nameserver）所登記的 **IPv4（A 紀錄）或 IPv6（AAAA 紀錄）位址**==。這項紀錄直接存放在上層網域註冊局（TLD Registry），用來解決 DNS 查詢時的循環依賴問題。 [[1](https://docs.gandi.net/zh-hant/domain_names/advanced_users/glue_records.html), [2](https://support.dnsimple.com/articles/what-are-glue-records/), [3](https://www.ibm.com/think/topics/glue-records)]

DNS 膠合紀錄（Glue Records）的核心運作

- **解決循環依賴。** 當網域 `example.com` 的名稱伺服器設為自家的子網域（例如 `://example.com`）時，解析器必須先知道 `://example.com` 的 IP 才能查詢網域資料，而 Glue IP 會在父層直接提供該 IP 作為引導。 [[1](https://support.dnsimple.com/articles/what-are-glue-records/), [2](https://docs.gandi.net/zh-hant/domain_names/advanced_users/glue_records.html)]
- **設定與生效位置。** 需透過網域註冊商（Registrar）後台提交名稱伺服器主機名稱與對應的 IP，資料會同步寫入頂級網域（TLD）的區域檔中。 [[1](https://www.cloudns.net/wiki/article/295/), [2](https://support.dnsimple.com/articles/what-are-glue-records/)]
- **驗證與排錯。** 可使用 `dig` 指令直接查詢頂級網域伺服器，確認 `ADDITIONAL SECTION` 是否回傳正確的 IP，設定變更後的傳播時間通常需數分鐘至 48 小時。 [[1](https://www.ibm.com/think/topics/glue-records), [2](https://www.cloudns.net/wiki/article/295/)]

其他常見的「Glue IP」技術情境

- **AWS Glue 子網路 IP 配置。** 在雲端資料整合服務 [AWS Glue](https://aws.amazon.com/blogs/big-data/scale-aws-glue-jobs-by-optimizing-ip-address-consumption-and-expanding-network-capacity-using-a-private-nat-gateway/) 中，每個執行作業的資料處理單元（DPU）都會在 VPC 子網路中建立一個彈性網路介面（ENI）並佔用一個可用 IP。若子網路 IP 耗盡會導致作業失敗，可透過啟用 Auto Scaling 或擴充 CIDR 網段來解決。 [[1](https://aws.amazon.com/blogs/big-data/scale-aws-glue-jobs-by-optimizing-ip-address-consumption-and-expanding-network-capacity-using-a-private-nat-gateway/), [2](https://repost.aws/zh-Hant/knowledge-center/glue-specified-subnet-free-addresses)]
- **IT Glue 存取控制（IP Access Control）。** 在 IT 文件管理平台 [IT Glue](https://www.itglue.com/features/ip-access-control/) 中，此功能是指限制特定 IP 位址或網段才能登入系統的安全白名單機制。 [[1](https://www.itglue.com/features/ip-access-control/)]

請問您目前想了解哪一種情境的 **Glue IP** 設定或排錯？

- **DNS 膠合紀錄（Glue Records）**（例如如何幫自架名稱伺服器綁定 IP，或解決解析失敗）
- **AWS Glue 網路與 IP 耗盡問題**（例如調整 VPC 子網路或 DPU 數量）
- **IT Glue 的 IP 白名單限制**

請告訴我您的**使用平台或網域註冊商**，我可以為您提供具體的操作步驟。