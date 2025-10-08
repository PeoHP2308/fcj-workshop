---
title: "Blog Translated"
date: "2025-10-06"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---



# Xây dựng xác minh vị trí địa lý cho iGaming và cá cược thể thao trên AWS

*Bởi Tiến sĩ Mike Reaves và Tiến sĩ Haowen You, ngày 04 tháng 9 năm 2025, trong Amazon CloudFront, Amazon CloudWatch, Amazon Cognito, Amazon Simple Storage Service (S3), AWS WAF, Cá cược và Trò chơi, Trò chơi, Ngành công nghiệp, Công cụ quản lý, Mạng & Phân phối nội dung, Bảo mật, Danh tính & Tuân thủ, Lưu trữ*

---


Xác minh vị trí địa lý trong cá cược thể thao và iGaming phục vụ hai mục đích chính: **tuân thủ** và **ngăn chặn gian lận**. Các nhà vận hành cá cược trực tuyến và iGaming có thể cần đảm bảo rằng chỉ những người chơi từ một số khu vực nhất định trên thế giới mới có thể xem nội dung do hạn chế về giấy phép tại thị trường. Hoặc quy định trò chơi có thể yêu cầu rằng quyền truy cập chỉ được giới hạn cho những người trong phạm vi pháp lý được phép, đồng thời chặn lưu lượng trái phép tại các biên giới địa chính trị.

Tại Hoa Kỳ, <u>Đạo luật Liên bang về Truyền thông Cược qua Đường dây</u> , cấm cá cược thể thao xuyên tiểu bang, bắt đầu bằng văn bản sau:

> Bất kỳ ai tham gia vào hoạt động cá cược hoặc đặt cược mà cố ý sử dụng phương tiện liên lạc bằng dây để truyền tải trong thương mại liên bang hoặc quốc tế các cược hoặc đặt cược, hoặc thông tin hỗ trợ việc đặt cược hoặc đặt cược vào bất kỳ sự kiện hoặc cuộc thi thể thao nào, hoặc để truyền tải thông tin bằng dây cho phép người nhận nhận tiền hoặc tín dụng do các cược hoặc đặt cược đó, hoặc thông tin hỗ trợ việc đặt cược hoặc đặt cược, sẽ bị phạt theo điều khoản này hoặc bị phạt tù không quá hai năm, hoặc cả hai.

IGT và Xổ số Bang New Hampshire đã <u>thắng kiện</u> Bộ Tư pháp Hoa Kỳ (US) liên quan đến việc Đạo luật Wire không áp dụng cho hoạt động cờ bạc trực tuyến (iGaming) và xổ số. Tuy nhiên, một số tiểu bang vẫn yêu cầu người chơi và cơ sở hạ tầng cá cược phải nằm trong cùng một tiểu bang. Tại những tiểu bang này, các nhà điều hành cũng phải thiết lập các cơ chế để phát hiện mạng riêng ảo (VPN), phần mềm, ảo hóa và các phương pháp khác có khả năng vượt qua việc phát hiện vị trí người chơi. Thông thường, điều này có nghĩa là phải biên dịch SDK thành một ứng dụng hoặc chạy phần mềm khác ( <u>sidecar</u> ) cùng với nó .

Ngoài việc cho phép nhà điều hành tuân thủ các quy định, kiểm soát truy cập theo địa lý còn mang lại lợi ích kinh doanh bổ sung. Việc chặn lưu lượng truy cập tại vùng biên giúp giảm chi phí cơ sở hạ tầng bằng cách loại bỏ lưu lượng truy cập trái phép trước khi chúng đến được hệ thống lõi. Các biện pháp kiểm soát này cũng có thể được sử dụng để ngăn chặn gian lận, ví dụ như cái gọi là cá cược ủy nhiệm, trong đó người chơi trong một khu vực địa lý được phép đặt cược cho nhiều người bên ngoài khu vực đó.

Việc lựa chọn công nghệ xác minh định vị địa lý phụ thuộc vào trường hợp sử dụng cụ thể của công ty. Tại Hoa Kỳ, Brazil và một số khu vực khác, các nhà điều hành cá cược thể thao và iGaming sử dụng các hệ thống định vị địa lý được cấp phép và đã được kiểm tra tuân thủ, với SDK truy cập trực tiếp vào hệ điều hành di động để đáp ứng các yêu cầu chống giả mạo. Tuy nhiên, có những lựa chọn thay thế có thể phù hợp với các nhà cung cấp nội dung hoặc nhà điều hành ở các khu vực khác.

Các phương pháp này có thể được sử dụng riêng lẻ hoặc kết hợp với nhau để mang lại giải pháp thỏa đáng, tùy thuộc vào nhu cầu tuân thủ và kinh doanh. Hãy cùng khám phá năm phương pháp riêng biệt mà các công ty có thể sử dụng để xây dựng và triển khai xác minh vị trí địa lý bằng Amazon Web Service (AWS).

---

## 1. Chặn vị trí địa lý bằng Amazon Route 53

Khả năng <u>định tuyến vị trí địa lý</u> của <u>Amazon Route 53</u> có thể hạn chế quyền truy cập vào nội dung theo quốc gia, châu lục hoặc cấp bang Hoa Kỳ ở mức máy chủ hệ thống tên miền (DNS). Cách tiếp cận dựa trên DNS này xử lý quyết định vị trí trước khi lưu lượng truy cập đến cơ sở hạ tầng của bạn, giảm tải không cần thiết và chi phí liên quan. Một lợi ích chính của giải pháp này là nó không yêu cầu chỉnh sửa các cơ sở mã máy chủ hoặc khách hàng hiện có.

Việc triển khai sử dụng bản ghi DNS với các chính sách định tuyến vị trí địa lý, xác định vị trí của người dùng và phản hồi theo các quy tắc được cấu hình cho địa lý đó. Sau đây là một ví dụ chính sách định tuyến lưu lượng theo vị trí địa lý của Route 53:

```json
{
 "RuleType": "geo",
 "Locations": [
   {
     "EvaluateTargetHealth": true,
     "Country": "SE",
     "EndpointReference": "ENDPOINT_IF_TRAFFIC_ORIGINATES_FROM_SWEDEN"
   },
   {
     "Country": "*",
     "IsDefault": true,
     "EvaluateTargetHealth": true,
     "EndpointReference": "ENDPOINT_FOR_ALL_OTHER_TRAFFIC"
   }
 ]
}
```

Chính sách ví dụ định tuyến lưu lượng đến các điểm cuối dựa trên việc quốc gia gốc có phải là Thụy Điển hay không. Trong ví dụ này, lưu lượng bắt nguồn từ Thụy Điển sẽ được gửi đến một bộ cân bằng tải ứng dụng, trong khi lưu lượng từ nơi khác sẽ được chuyển hướng đến một trang lỗi được lưu trữ trên <u>Amazon CloudFront</u>. Định tuyến vị trí địa lý của Route 53 tích hợp trực tiếp với các dịch vụ AWS khác và bao gồm các quy tắc dự phòng cho các vị trí không khớp.

Định vị địa lý dựa trên DNS có những hạn chế. Giải pháp này dựa vào độ chính xác của việc ánh xạ IP tới vị trí, có thể bị vượt qua bởi VPN hoặc máy chủ proxy. Các phản hồi DNS có thể được bộ phân giải trung gian lưu vào bộ nhớ cache, cho phép người dùng tiếp tục truy cập từ vị trí bị chặn sau khi di chuyển. Ngoài ra, giải pháp này không thể phát hiện giả mạo thiết bị, giả mạo vị trí hoặc ngăn chặn cá cược ủy quyền. Nó chưa được phê duyệt cho việc sử dụng bởi các nhà điều hành cờ bạc trực tuyến có giấy phép tại Hoa Kỳ nhằm đáp ứng nhu cầu tuân thủ của họ.

---

## 2. Amazon Location Service với JavaScript

Người dùng có thể sử dụng <u>Amazon Location Service</u> kết hợp với JavaScript phía client để xây dựng khả năng định vị địa lý trực tiếp bên trong bất kỳ ứng dụng nào dùng JavaScript (ứng dụng web, React Native, Swift, v.v.). Cách tiếp cận này lấy tọa độ GPS từ môi trường của người dùng, có thể mang lại độ chính xác vị trí cao hơn so với các phương pháp dựa trên IP. Các nhà phát triển phải chỉnh sửa ứng dụng của họ để thêm mã yêu cầu vị trí, tích hợp <u>AWS SDK cho JavaScript (phiên bản 3)</u>, và tạo API endpoint để xác minh vị trí.
Đối với một triển khai cơ bản, sau đây là một ví dụ về triển khai JavaScript có xử lý lỗi:

```javascript
// use API Key id for credentials
const authHelper = await withAPIKey("api-key-id"); 

// Initialize the Location client
const locationClient = new LocationClient({ 
    region: "us-east-1",
    ...authHelper.getLocationClientConfig()
});

async function updateDeviceLocation() {
    try {
        const position = await new Promise((resolve, reject) => {
            navigator.geolocation.getCurrentPosition(resolve, reject, {
                enableHighAccuracy: true,
                timeout: 10000
            });
        });

        const command = new BatchUpdateDevicePositionCommand({
            TrackerName: "MyDeviceTracker",
            Updates: [{
                DeviceId: "mobile-device-123",
                Position: [position.coords.longitude, position.coords.latitude],
                SampleTime: new Date()
            }]
        });

        const response = await locationClient.send(command);
        console.log("Location updated successfully:", response);
        
    } catch (error) {
        console.error("Failed to update location:", error);
    }
} 
```

Dịch vụ Định vị Amazon cung cấp một <u>trình trợ giúp xác thực JavaScript</u> để đơn giản hóa quá trình xác thực khi thực hiện các lệnh gọi API. Bằng cách này, bạn có thể tránh việc mã hóa cứng thông tin xác thực trong JavaScript. Bạn có thể sử dụng <u>Amazon Cognito</u> hoặc khóa API làm phương thức xác thực. Giải pháp này yêu cầu tương tác của người dùng. Trình duyệt sẽ hiển thị lời nhắc cấp quyền yêu cầu truy cập vào dịch vụ định vị. Sau khi được cấp quyền, mã sẽ truy xuất tọa độ GPS và phát ra các sự kiện khi vượt qua ranh giới hàng rào địa lý.

Nếu sử dụng `ForecastGeofenceEvents` lệnh gọi API, Amazon Location Service có thể xác thực chúng dựa trên các ranh giới địa lý đã xác định. Đối với việc triển khai Android WebView, nhà phát triển phải bật JavaScript trong cài đặt WebView bằng cách yêu cầu `ACCESS_FINE_LOCATION` cấp quyền trong tệp kê khai ứng dụng. Họ cũng phải triển khai một tùy chỉnh `WebChromeClient` để xử lý các yêu cầu cấp quyền định vị địa lý. Các ứng dụng Android cũng yêu cầu bộ nhớ đệm cục bộ trên thiết bị để lưu trữ quyền định vị địa lý và vị trí, được cấu hình thông qua cài đặt đường dẫn cơ sở dữ liệu định vị địa lý của WebView.

Đối với các triển khai iOS WebView sử dụng `WKWebView`, định vị địa lý được cho phép theo mặc định, nhưng nhà phát triển phải thêm `NSLocationWhenInUseUsageDescription` khóa vào `Info.plist` tệp của họ kèm theo thông báo giải thích yêu cầu truy cập vị trí. Ứng dụng cũng phải yêu cầu cấp phép vị trí thông qua khung dịch vụ định vị iOS.

Giải pháp này chỉ khả thi cho các ứng dụng sử dụng JavaScript. Việc triển khai này gây ra độ trễ do hệ thống phải chờ sự cho phép của người dùng và thu thập dữ liệu GPS. Trải nghiệm người dùng có thể bị ảnh hưởng khi quyền truy cập vị trí bị từ chối hoặc tín hiệu GPS không khả dụng trong một số môi trường nhất định. Phương pháp này không thể phát hiện giả mạo vị trí hoặc can thiệp vào thiết bị. Mặc dù phương pháp này cung cấp dữ liệu vị trí chính xác hơn so với các giải pháp dựa trên IP, nhưng vẫn chưa đáp ứng được các yêu cầu nghiêm ngặt hơn đối với các hoạt động trò chơi được quản lý, đặc biệt là khi cần phát hiện giả mạo vị trí.

---

## 3. Chặn hoặc cho phép qua Amazon CloudFront

Giới hạn địa lý mà Amazon CloudFront cung cấp là kiểm soát truy cập nội dung tại các vị trí biên của AWS. Giải pháp này chặn hoặc cho phép các yêu cầu trước khi chúng đến máy chủ gốc của bạn, giúp giảm cả chi phí cơ sở hạ tầng và rủi ro bảo mật tiềm ẩn.

Sau đây là cấu hình giới hạn địa lý của CloudFront được sử dụng trong Cài đặt phân phối:

```json
{
  "GeoRestriction": {
    "RestrictionType": "allowlist",
    "Locations": ["GB", "IE", "MT"]
  }
}
```

Bạn cũng có thể <u>cấu hình CloudFront để thêm tiêu đề vị trí</u> vào các yêu cầu mà CloudFront nhận được từ người dùng và chuyển tiếp chúng đến ứng dụng của bạn. Bằng cách này, bạn có thể tạo logic ứng dụng có thể thực hiện các hành động khác ngoài việc cho phép hoặc từ chối quyền truy cập của người dùng. Giới hạn địa lý của CloudFront chỉ áp dụng ở cấp độ quốc gia—chúng không thể phân biệt giữa các tiểu bang hoặc khu vực của Hoa Kỳ trong cùng một quốc gia.

Đối với các nhà khai thác tại các thị trường yêu cầu chặn theo quốc gia, CloudFront cung cấp tuyến phòng thủ đầu tiên tiết kiệm chi phí. Tính năng chặn theo địa lý của CloudFront chỉ cần chi phí phát triển tối thiểu. Tính năng này có thể được cấu hình thông qua AWS Console, giao diện dòng lệnh (CLI) hoặc các công cụ và dịch vụ Cơ sở hạ tầng dưới dạng Mã (IaC) mà không cần sửa đổi mã ứng dụng. Điều này khiến CloudFront trở thành một lựa chọn hấp dẫn để triển khai nhanh chóng các biện pháp kiểm soát địa lý cơ bản.

Việc sử dụng các hạn chế địa lý của CloudFront không phát sinh thêm chi phí nào ngoài phí sử dụng CloudFront tiêu chuẩn. Các yêu cầu bị chặn sẽ bị từ chối tại vị trí biên, giúp giảm thiểu chi phí truyền dữ liệu. Khi một yêu cầu bị chặn, CloudFront sẽ trả về phản hồi lỗi HTTP 403 (Bị cấm), chỉ tiêu tốn vài byte dữ liệu truyền. Điều này làm cho việc chặn địa lý của CloudFront đặc biệt tiết kiệm cho các ứng dụng nhận lưu lượng truy cập lớn từ các khu vực bị hạn chế.

Việc chặn các yêu cầu tại các vị trí biên CloudFront thay vì máy chủ gốc của bạn có thể tiết kiệm đáng kể chi phí băng thông và giảm tải cho cơ sở hạ tầng backend. Ngoài ra, vì các cuộc tấn công từ chối dịch vụ phân tán (DDoS) thường xuất phát từ các khu vực địa lý cụ thể, việc chặn ở cấp độ quốc gia có thể giảm thiểu rủi ro bị tấn công và chi phí liên quan.

Tuy nhiên, tính năng chặn địa lý của CloudFront có những hạn chế vượt ra ngoài phạm vi chi tiết ở cấp độ quốc gia. Dịch vụ này dựa vào ánh xạ địa chỉ IP để xác định vị trí, điều này có thể bị vượt qua bởi VPN hoặc máy chủ proxy. Nó dựa vào các tiêu đề kiểm soát bộ nhớ đệm và các chiến lược vô hiệu hóa để xác minh độ mới của dữ liệu cho các API động—có nghĩa là dữ liệu vị trí có thể không phải lúc nào cũng được cập nhật. Ngoài ra, phương pháp này không có sẵn các biện pháp kiểm tra tính toàn vẹn của thiết bị hoặc chống giả mạo, điều này có thể khiến nó không còn là một lựa chọn cho các nhà điều hành iGaming hoặc cá cược thể thao có yêu cầu định vị địa lý nghiêm ngặt hơn.

---

## 4. Các câu lệnh khớp địa lý AWS WAF

<u>AWS WAF</u> cung cấp khả năng kiểm soát truy cập theo địa lý thông qua các câu lệnh khớp địa lý, mang lại khả năng kiểm soát chi tiết hơn so với các hạn chế của CloudFront. Dịch vụ này có thể lọc lưu lượng dựa trên vị trí quốc gia và tiểu bang Hoa Kỳ, rất hữu ích cho các nhà điều hành cần kiểm soát truy cập chính xác theo khu vực.

Sau đây là quy tắc AWS WAF với câu lệnh khớp địa lý:

```json
{
  "Name": "RegionalAccessControl",
  "Statement": {
    "GeoMatchStatement": {
      "CountryCodes": ["SG"],
      "ForwardedIPConfig": {
        "HeaderName": "X-Forwarded-For",
        "FallbackBehavior": "MATCH"
      }
    }
  },
  "Action": { "Allow": {} },
  "VisibilityConfig": {
    "SampledRequestsEnabled": true,
    "CloudWatchMetricsEnabled": true,
    "MetricName": "RegionalAccessMetric"
  }
}
```

Một lợi thế quan trọng của việc khớp địa lý AWS WAF là tích hợp với số liệu của <u>Amazon CloudWatch</u> , cho phép giám sát chi tiết các yêu cầu bị chặn và các kiểu lưu lượng. Khả năng hiển thị này giúp các nhà điều hành hiểu rõ các kiểu truy cập và điều chỉnh chiến lược chặn của họ cho phù hợp. AWS WAF cũng xử lý lưu lượng IPv6 gốc và có thể xử lý các yêu cầu dựa trên `X-Forwarded-For` tiêu đề, điều này rất quan trọng đối với các ứng dụng phía sau máy chủ proxy hoặc bộ cân bằng tải.

Tuy nhiên, tính năng so khớp địa lý AWS WAF không thể phát hiện các nỗ lực giả mạo vị trí tinh vi hoặc xác minh tính toàn vẹn của thiết bị. Mặc dù có thể <u>xác định và chặn lưu lượng truy cập từ các dịch vụ proxy và điểm cuối VPN đã biết</u> , người dùng vẫn có thể lách luật kiểm soát này.

---

## 5. Nhà cung cấp dịch vụ xác minh vị trí địa lý được cấp phép

Có nhiều khu vực pháp lý có yêu cầu nghiêm ngặt đối với hệ thống xác minh vị trí địa lý (ví dụ: Hoa Kỳ và Brazil). Đối với các nhà điều hành trò chơi được quản lý, những thị trường này thường yêu cầu hệ thống xác minh vị trí địa lý có khả năng phát hiện và ngăn chặn giả mạo vị trí bằng cách xác minh tính toàn vẹn ở cấp độ thiết bị. Các nhà cung cấp xác minh vị trí địa lý được cấp phép như <u>GeoComply</u> , <u>OpenBet</u> và <u>Xpoint</u> cung cấp các khả năng này.

Giải pháp của các công ty này khác biệt cơ bản so với các giải pháp đã mô tả trước đây về phương pháp xác minh vị trí. Thay vì chỉ dựa vào địa chỉ IP hoặc tọa độ GPS, họ lấy thông tin vị trí từ chính thiết bị (sử dụng phép đo ba chiều Wi-Fi, GPS, v.v.). Họ xác minh tính toàn vẹn của thiết bị bằng cách sử dụng SDK dành riêng cho thiết bị để kiểm tra hệ điều hành xem có dấu hiệu giả mạo, VPN hoặc phần mềm giả mạo vị trí hay không.

Hệ thống quản lý hàng rào địa lý xác định xem thiết bị có nằm trong hoặc gần ranh giới hàng rào địa lý hay không, có thể là biên giới tiểu bang hoặc ranh giới chính trị khác. Giám sát theo thời gian thực phát hiện những thay đổi vị trí đột ngột, có thể cho thấy hành vi cá cược ủy nhiệm hoặc hành vi bất chính khác. Việc xây dựng các hệ thống này thường yêu cầu tích hợp ở nhiều cấp độ, bao gồm:

- Tích hợp SDK gốc (iOS và Android) cho ứng dụng di động
- Plugin trình duyệt hoặc ứng dụng sidecar cho ứng dụng web
- Tích hợp API Dịch vụ vị trí của Amazon
- Điểm cuối báo cáo tuân thủ

Các nhà cung cấp dịch vụ định vị địa lý được cấp phép là một lựa chọn tốt khi:

- Các quy định yêu cầu xác minh vị trí và hàng rào địa lý chính xác (chẳng hạn như Đạo luật chuyển tiền liên bang Hoa Kỳ hoặc luật trò chơi của Brazil)
- Các yêu cầu về quyền hạn bắt buộc phải chống giả mạo và xác minh tính toàn vẹn của thiết bị
- Các nhà điều hành cần ngăn chặn cá cược ủy quyền thông qua các kiểm tra cấp thiết bị tinh vi

---

## Kết luận

Việc lựa chọn phương pháp định vị địa lý phụ thuộc vào nhiều yếu tố, bao gồm các yêu cầu pháp lý đối với khu vực địa lý mục tiêu, hồ sơ rủi ro và chi phí. Mặc dù nhiều nhà khai thác triển khai nhiều lớp bảo vệ, điều quan trọng là phải hiểu giải pháp nào phù hợp, dựa trên các rủi ro và yêu cầu cụ thể. Nếu không yêu cầu các tiêu chí khắt khe, các giải pháp ít tốn kém hơn được nêu trước đây có thể cung cấp khả năng định vị địa lý hữu ích trong khi vẫn duy trì hiệu suất và trải nghiệm người dùng.