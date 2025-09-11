# Payment Verification Debug Guide

## Issue Summary
The payment verification is failing with a 400 Bad Request error when trying to verify payments through the `/api/payments/verify` endpoint.

## Root Causes Identified

### 1. Authentication Issues
- The payment verification endpoint requires authentication
- User's session might expire during the payment process
- Token might be missing or invalid when Razorpay callback is executed

### 2. Missing Environment Variables
- Razorpay credentials might not be properly configured
- Check if `RAZORPAY_KEY_ID` and `RAZORPAY_KEY_SECRET` are set

### 3. Payment Data Issues
- Payment ID might not exist in database
- Razorpay Order ID might be missing
- Signature verification might be failing

## Solutions Implemented

### Frontend Improvements
1. **Enhanced Error Handling**: Added specific error messages for different failure scenarios
2. **Token Validation**: Check if authentication token exists before making verification request
3. **Better User Feedback**: Provide clear error messages to users

### Backend Improvements
1. **Detailed Logging**: Added comprehensive logging for payment verification requests
2. **Better Validation**: Enhanced validation with specific error messages for each field
3. **Payment Existence Check**: Verify payment exists before attempting verification
4. **Improved Error Responses**: Return more detailed error information

## Debugging Steps

### 1. Check Environment Variables
```bash
# In backend directory
echo $RAZORPAY_KEY_ID
echo $RAZORPAY_KEY_SECRET
```

### 2. Check Database
```sql
-- Check recent payments
SELECT id, userId, amount, status, razorpayOrderId, createdAt 
FROM Payment 
ORDER BY createdAt DESC 
LIMIT 10;

-- Check if payment exists
SELECT * FROM Payment WHERE id = 'your-payment-id';
```

### 3. Check Backend Logs
Look for these log messages:
- "Payment verification request:"
- "Payment verification failed:"
- "Payment verification successful:"

### 4. Check Frontend Console
Look for these log messages:
- "Verifying payment with data:"
- "Payment verification response:"
- "Payment verification error:"

## Common Issues and Solutions

### Issue 1: "Authentication token not found"
**Solution**: User needs to log in again and retry payment

### Issue 2: "Payment not found"
**Solution**: Check if payment ID is correct and exists in database

### Issue 3: "Payment verification failed"
**Solution**: Check Razorpay credentials and signature verification

### Issue 4: "Razorpay is not configured"
**Solution**: Set up Razorpay environment variables

## Testing Payment Verification

### Manual Test
1. Create a payment order
2. Complete payment through Razorpay
3. Check browser console for verification logs
4. Check backend logs for verification process

### API Test
```bash
curl -X POST http://localhost:3001/api/payments/verify \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "paymentId": "payment-id",
    "razorpayPaymentId": "razorpay-payment-id",
    "razorpaySignature": "razorpay-signature"
  }'
```

## Environment Setup

### Required Environment Variables
```env
# Razorpay Configuration
RAZORPAY_KEY_ID="rzp_test_xxxxxxxxxxxxx"
RAZORPAY_KEY_SECRET="xxxxxxxxxxxxxxxxxxxxxxxx"

# Database
DATABASE_URL="postgresql://username:password@localhost:5432/kmun_db"

# JWT Secret
JWT_SECRET="your-super-secret-jwt-key-here"
```

## Next Steps

1. **Verify Environment Variables**: Ensure Razorpay credentials are properly set
2. **Test Payment Flow**: Try a complete payment flow and check logs
3. **Check Database**: Verify payment records are being created correctly
4. **Monitor Logs**: Watch both frontend and backend logs during payment verification

## Files Modified

### Frontend
- `frontend/src/components/Common/CompletePaymentButton.tsx`
- `frontend/src/components/Common/PaymentGateway.tsx`
- `frontend/src/services/api.ts`

### Backend
- `backend/src/controllers/paymentController.js`

## Support

If issues persist:
1. Check browser console for detailed error messages
2. Check backend logs for payment verification details
3. Verify Razorpay credentials are correct
4. Ensure database connection is working
5. Test with a fresh payment order
