nd gateway# Payment Gateway Setup Guide - Razorpay Integration

## 🚀 Complete Payment Gateway Configuration

This guide will help you set up the Razorpay payment gateway for K-MUN 2025 with complete transaction logging and admin dashboard integration.

## 📋 Prerequisites

1. **Razorpay Account**: Sign up at [razorpay.com](https://razorpay.com)
2. **Business Verification**: Complete KYC and business verification
3. **API Keys**: Generate test and live API keys

## 🔧 Environment Configuration

### Backend Environment Variables

Add these variables to your `backend/.env` file:

```env
# Razorpay Configuration
RAZORPAY_KEY_ID="rzp_test_xxxxxxxxxxxxx"
RAZORPAY_KEY_SECRET="xxxxxxxxxxxxxxxxxxxxxxxx"

# For Production (replace with live keys)
# RAZORPAY_KEY_ID="rzp_live_xxxxxxxxxxxxx"
# RAZORPAY_KEY_SECRET="xxxxxxxxxxxxxxxxxxxxxxxx"
```

### Frontend Environment Variables

Add these variables to your `frontend/.env` file:

```env
# API Configuration
VITE_API_URL=http://localhost:3001

# Razorpay Configuration (Optional - can be fetched from backend)
VITE_RAZORPAY_KEY_ID="rzp_test_xxxxxxxxxxxxx"
```

## 🏗️ Database Schema

The payment system uses the following database tables:

### Payment Table
```sql
CREATE TABLE "Payment" (
  "id" TEXT NOT NULL,
  "userId" TEXT NOT NULL,
  "registrationId" TEXT,
  "amount" DECIMAL(10,2) NOT NULL,
  "currency" TEXT NOT NULL DEFAULT 'INR',
  "status" TEXT NOT NULL DEFAULT 'PENDING',
  "razorpayOrderId" TEXT,
  "razorpayPaymentId" TEXT,
  "razorpaySignature" TEXT,
  "createdAt" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP,
  "updatedAt" TIMESTAMP(3) NOT NULL,
  CONSTRAINT "Payment_pkey" PRIMARY KEY ("id")
);
```

### Activity Log Table
```sql
CREATE TABLE "ActivityLog" (
  "id" TEXT NOT NULL,
  "userId" TEXT NOT NULL,
  "action" TEXT NOT NULL,
  "details" JSONB,
  "createdAt" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP,
  CONSTRAINT "ActivityLog_pkey" PRIMARY KEY ("id")
);
```

## 🔄 Payment Flow

### 1. Payment Initiation
```javascript
// Frontend: Create payment order
const orderResponse = await paymentsAPI.createOrder({
  userId: 'user123',
  registrationId: 'reg456',
  amount: 2500,
  currency: 'INR'
});
```

### 2. Razorpay Checkout
```javascript
// Frontend: Open Razorpay checkout
const options = {
  key: 'rzp_test_xxxxxxxxxxxxx',
  amount: orderResponse.data.razorpayOrder.amount,
  currency: 'INR',
  name: 'Kumaraguru MUN 2025',
  order_id: orderResponse.data.razorpayOrder.id,
  handler: function (response) {
    // Handle payment success
  }
};
const razorpay = new Razorpay(options);
razorpay.open();
```

### 3. Payment Verification
```javascript
// Frontend: Verify payment
const verifyResponse = await paymentsAPI.verifyPayment({
  paymentId: payment.id,
  razorpayPaymentId: response.razorpay_payment_id,
  razorpaySignature: response.razorpay_signature
});
```

## 📊 Admin Dashboard Features

### Transaction Records Tab
- **Payment Statistics**: Total payments, success rate, amounts
- **Payment List**: All transactions with filters and search
- **Transaction Logs**: Detailed activity logs
- **Real-time Updates**: Refresh functionality

### Key Features
- ✅ **Payment Status Tracking**: PENDING, PAID, FAILED, REFUNDED
- ✅ **User Information**: Name, email, institution
- ✅ **Amount Details**: Currency, amounts, totals
- ✅ **Date/Time Tracking**: Creation and update timestamps
- ✅ **Search & Filter**: By status, user, date range
- ✅ **Pagination**: Handle large transaction volumes
- ✅ **Export Functionality**: Download transaction reports

## 🔐 Security Features

### 1. Payment Verification
- **Signature Verification**: All payments verified using Razorpay signatures
- **Order Validation**: Ensures payment matches original order
- **Amount Validation**: Prevents amount tampering

### 2. Activity Logging
- **All Actions Logged**: Every payment action is recorded
- **User Tracking**: Track who performed each action
- **Detailed Logs**: Complete transaction details stored
- **Audit Trail**: Full payment history maintained

### 3. Error Handling
- **Graceful Failures**: Proper error handling and user feedback
- **Retry Logic**: Handle network failures and timeouts
- **Validation**: Input validation on all payment data

## 🧪 Testing

### Test Mode Setup
1. Use Razorpay test keys
2. Test with Razorpay test cards
3. Verify all payment flows
4. Test error scenarios

### Test Cards
```
Success: 4111 1111 1111 1111
Failure: 4000 0000 0000 0002
CVV: Any 3 digits
Expiry: Any future date
```

## 🚀 Production Deployment

### 1. Live Keys
- Replace test keys with live keys
- Update environment variables
- Test with small amounts first

### 2. Webhook Configuration
```javascript
// Configure webhooks in Razorpay dashboard
// URL: https://yourdomain.com/api/payments/webhook
// Events: payment.captured, payment.failed
```

### 3. SSL Certificate
- Ensure HTTPS is enabled
- Razorpay requires secure connections
- Update API URLs to use HTTPS

## 📱 Frontend Integration

### Payment Component Usage
```jsx
import PaymentGateway from '../components/Common/PaymentGateway';

<PaymentGateway
  userId={user.id}
  registrationId={registration.id}
  amount={2500}
  onSuccess={(paymentData) => {
    // Handle successful payment
    console.log('Payment successful:', paymentData);
  }}
  onFailure={(error) => {
    // Handle payment failure
    console.error('Payment failed:', error);
  }}
/>
```

## 🔍 Monitoring & Analytics

### Key Metrics to Track
- **Payment Success Rate**: Percentage of successful payments
- **Average Transaction Value**: Mean payment amount
- **Payment Methods**: Popular payment options
- **Geographic Distribution**: Payment locations
- **Time-based Analytics**: Peak payment times

### Dashboard Metrics
- Total payments count
- Successful payments count
- Total amount collected
- Success rate percentage
- Pending payments count
- Failed payments count

## 🛠️ Troubleshooting

### Common Issues

1. **Payment Not Captured**
   - Check Razorpay dashboard
   - Verify webhook configuration
   - Check signature verification

2. **Test Payments Not Working**
   - Verify test API keys
   - Check test card numbers
   - Ensure test mode is enabled

3. **Webhook Not Receiving**
   - Check webhook URL accessibility
   - Verify SSL certificate
   - Check webhook event configuration

### Debug Mode
```javascript
// Enable debug logging
console.log('Payment order:', orderResponse);
console.log('Payment verification:', verifyResponse);
```

## 📞 Support

### Razorpay Support
- **Documentation**: [razorpay.com/docs](https://razorpay.com/docs)
- **Support**: [support@razorpay.com](mailto:support@razorpay.com)
- **Status Page**: [status.razorpay.com](https://status.razorpay.com)

### Implementation Support
- Check server logs for detailed error messages
- Verify database connections
- Test API endpoints individually
- Use browser developer tools for frontend debugging

## ✅ Checklist

- [ ] Razorpay account created and verified
- [ ] API keys generated and configured
- [ ] Environment variables set
- [ ] Database schema updated
- [ ] Payment flow tested
- [ ] Admin dashboard integrated
- [ ] Transaction logging working
- [ ] Error handling implemented
- [ ] SSL certificate configured
- [ ] Webhooks configured
- [ ] Production keys ready
- [ ] Monitoring setup

## 🎯 Next Steps

1. **Complete Setup**: Follow all configuration steps
2. **Test Thoroughly**: Test all payment scenarios
3. **Deploy to Production**: Use live keys and HTTPS
4. **Monitor Performance**: Track payment success rates
5. **Regular Updates**: Keep Razorpay SDK updated
6. **Backup Strategy**: Regular database backups
7. **Security Review**: Regular security audits

The payment gateway is now fully configured with comprehensive logging and admin dashboard integration!
