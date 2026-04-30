# PeterPay API Documentation 🚀

[![GitHub stars](https://img.shields.io/github/stars/King-pe/PeterPay-API-Documentation?style=for-the-badge&color=yellow)](https://github.com/King-pe/PeterPay-API-Documentation/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/King-pe/PeterPay-API-Documentation?style=for-the-badge&color=blue)](https://github.com/King-pe/PeterPay-API-Documentation/network/members)
[![Donate](https://img.shields.io/badge/Donate-Orange?style=for-the-badge&logo=paypal&logoColor=white)](https://www.peterpay.link/pay?code=844ac71e)

Karibu kwenye mwongozo wa PeterPay API. Mfumo huu unakuwezesha kupokea malipo ya simu (Mobile Money) moja kwa moja kwenye tovuti au mfumo wako kwa urahisi na usalama.

---

## 📌 Utangulizi
PeterPay API inakuwezesha kuunganisha malipo ya simu (M-Pesa, Tigo Pesa, Airtel Money, n.k.) kwenye application yako.

- **Base URL:** `https://www.peterpay.link/api/v1`
- **Dashboard:** [Pata API Key Hapa](https://www.peterpay.link/dashboard.php)

---

## 🔐 Authentication
Maombi yote ya API yanapaswa kuwa na `X-API-KEY` kwenye header. Unaweza kupata API Key yako kwenye Dashboard.

**Header Example:**
```http
X-API-KEY: pk_live_xxxxxxxxxxxxxxxxxxxxxxxx
```

---

## 💳 1. Create Order (Kuanzisha Malipo)
Tumia endpoint hii kuanzisha muamala wa malipo. Mteja atapokea ombi la USSD (Push) kwenye simu yake.

- **Method:** `POST`
- **Endpoint:** `/create_order`

### Parameters
| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `amount` | Float | Yes | Kiasi cha pesa (Min: 500 TZS). |
| `buyer_phone` | String | Yes | Namba ya simu ya mteja (255...). |
| `buyer_name` | String | No | Jina la mteja. |
| `buyer_email` | String | No | Barua pepe ya mteja. |

### Mfano wa Request (PHP)
```php
$url = 'https://www.peterpay.link/api/v1/create_order';
$data = [
    'amount' => 1000,
    'buyer_phone' => '255753033342',
    'buyer_name' => 'Peter Joram',
    'buyer_email' => 'john@example.com'
];

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Content-Type: application/json',
    'X-API-KEY: pk_live_xxxxxxxx'
]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($ch);
curl_close($ch);

print_r(json_decode($response, true));
```

### Success Response
```json
{
  "status": "success",
  "message": "Order created",
  "data": {
    "order_id": "API-ABC123XYZ",
    "payment_status": "PENDING"
  }
}
```

---

## 🔍 2. Check Status (Kukagua Hali ya Malipo)
Tumia endpoint hii kukagua kama muamala umekamilika au la.

- **Method:** `POST`
- **Endpoint:** `/order_status`

**Request Body:**
```json
{
  "order_id": "API-ABC123XYZ"
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "order_id": "API-ABC123XYZ",
    "payment_status": "COMPLETED",
    "amount": 1000,
    "currency": "TZS",
    "reference": "SP123456789"
  }
}
```

---

## 🪝 3. Webhooks (Callbacks)
Tunatuma taarifa kwenye server yako papo hapo malipo yanapokamilika. Hakikisha umeweka Webhook URL yako kwenye Dashboard.

### Payload Example
```json
{
  "event": "payment.completed",
  "order_id": "API-ABC123XYZ",
  "amount": 1000,
  "currency": "TZS",
  "status": "COMPLETED",
  "customer_phone": "255712345678",
  "timestamp": "2024-02-14T10:30:00+03:00"
}
```

### Security (Signature Verification)
Tunatuma header ya `X-PeterPay-Signature` ambayo ni HMAC SHA256 ya payload kwa kutumia **API Secret** yako.

```php
$payload = file_get_contents('php://input');
$signature = $_SERVER['HTTP_X_PETERPAY_SIGNATURE'];
$secret = 'YOUR_API_SECRET';

$calculated = hash_hmac('sha256', $payload, $secret);

if (hash_equals($calculated, $signature)) {
    // Ombi ni halali
}
```

---

## 🤝 Kuchangia (Support)
Ikiwa unataka kusaidia maendeleo ya mfumo huu, unaweza kutoa mchango wako hapa:

[![Donate](https://img.shields.io/badge/Donate-Orange?style=for-the-badge&logo=paypal&logoColor=white)](https://www.peterpay.link/pay?code=844ac71e)

---

## 👥 Contributors
Shukrani kwa wote waliochangia kwenye mradi huu!

<a href="https://github.com/King-pe/PeterPay-API-Documentation/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=King-pe/PeterPay-API-Documentation" />
</a>

---

**Developed by [Peter Joram](https://www.peterpay.link)**
