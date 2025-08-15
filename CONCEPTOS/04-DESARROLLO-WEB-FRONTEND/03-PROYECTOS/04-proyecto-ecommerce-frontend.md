# 🛒 Proyecto: E-commerce Frontend Completo

## 🎯 **OBJETIVO**
Crear una tienda online completa y funcional que demuestre el dominio de frontend moderno, con carrito de compras, sistema de usuarios, catálogo de productos y checkout optimizado.

---

## 📋 **DESCRIPCIÓN DEL PROYECTO**

### **Visión General**
Desarrollar una plataforma de e-commerce con interfaz moderna, funcionalidades avanzadas de compra y experiencia de usuario optimizada para conversión.

### **Características Principales**
- **Catálogo de Productos**: Grid responsive con filtros avanzados
- **Carrito de Compras**: Gestión de estado y persistencia local
- **Sistema de Usuarios**: Registro, login y perfiles
- **Proceso de Checkout**: Flujo de compra optimizado
- **Búsqueda Inteligente**: Filtros y ordenamiento
- **Wishlist**: Lista de deseos personal
- **Reviews y Ratings**: Sistema de valoraciones
- **Responsive Design**: Mobile-first approach

---

## 🏗️ **ESTRUCTURA DEL PROYECTO**

### **Organización de Archivos**
```
ecommerce-frontend/
├── public/
│   ├── index.html
│   ├── manifest.json
│   ├── robots.txt
│   ├── sitemap.xml
│   └── images/
│       ├── products/
│       ├── banners/
│       ├── icons/
│       └── logos/
├── src/
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Header.jsx
│   │   │   ├── Footer.jsx
│   │   │   ├── Navigation.jsx
│   │   │   └── Sidebar.jsx
│   │   ├── product/
│   │   │   ├── ProductCard.jsx
│   │   │   ├── ProductGrid.jsx
│   │   │   ├── ProductDetail.jsx
│   │   │   ├── ProductFilters.jsx
│   │   │   ├── ProductSearch.jsx
│   │   │   └── ProductReviews.jsx
│   │   ├── cart/
│   │   │   ├── CartItem.jsx
│   │   │   ├── CartSummary.jsx
│   │   │   ├── CartDrawer.jsx
│   │   │   └── CartBadge.jsx
│   │   ├── checkout/
│   │   │   ├── CheckoutForm.jsx
│   │   │   ├── PaymentForm.jsx
│   │   │   ├── OrderSummary.jsx
│   │   │   └── CheckoutSteps.jsx
│   │   ├── user/
│   │   │   ├── LoginForm.jsx
│   │   │   ├── RegisterForm.jsx
│   │   │   ├── UserProfile.jsx
│   │   │   ├── OrderHistory.jsx
│   │   │   └── Wishlist.jsx
│   │   └── common/
│   │       ├── Button.jsx
│   │       ├── Modal.jsx
│   │       ├── Input.jsx
│   │       ├── Select.jsx
│   │       ├── Loading.jsx
│   │       └── Notification.jsx
│   ├── hooks/
│   │   ├── useCart.js
│   │   ├── useProducts.js
│   │   ├── useAuth.js
│   │   ├── useWishlist.js
│   │   ├── useCheckout.js
│   │   └── useLocalStorage.js
│   ├── context/
│   │   ├── CartContext.jsx
│   │   ├── AuthContext.jsx
│   │   ├── ProductContext.jsx
│   │   └── ThemeContext.jsx
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── Products.jsx
│   │   ├── ProductDetail.jsx
│   │   ├── Cart.jsx
│   │   ├── Checkout.jsx
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── Profile.jsx
│   │   └── Orders.jsx
│   ├── data/
│   │   ├── products.js
│   │   ├── categories.js
│   │   └── mockData.js
│   ├── styles/
│   │   ├── components/
│   │   ├── layout/
│   │   ├── themes/
│   │   └── main.css
│   ├── utils/
│   │   ├── api.js
│   │   ├── helpers.js
│   │   ├── validators.js
│   │   └── constants.js
│   ├── App.jsx
│   └── index.js
├── package.json
└── README.md
```

---

## 🛒 **COMPONENTES PRINCIPALES**

### **1. ProductCard.jsx (Tarjeta de Producto)**
```jsx
import React, { useState } from 'react';
import { Link } from 'react-router-dom';
import { useCart } from '../../hooks/useCart';
import { useWishlist } from '../../hooks/useWishlist';
import { formatPrice, calculateDiscount } from '../../utils/helpers';
import './ProductCard.css';

const ProductCard = ({ product, variant = 'default' }) => {
  const [imageLoaded, setImageLoaded] = useState(false);
  const [isHovered, setIsHovered] = useState(false);
  
  const { addToCart, isInCart, getCartQuantity } = useCart();
  const { addToWishlist, removeFromWishlist, isInWishlist } = useWishlist();

  const {
    id,
    name,
    price,
    originalPrice,
    images,
    category,
    rating,
    reviewCount,
    stock,
    discount,
    tags
  } = product;

  const cartQuantity = getCartQuantity(id);
  const isWishlisted = isInWishlist(id);
  const hasDiscount = originalPrice && originalPrice > price;
  const discountPercentage = hasDiscount ? calculateDiscount(originalPrice, price) : 0;

  const handleAddToCart = () => {
    addToCart(product, 1);
  };

  const handleWishlistToggle = () => {
    if (isWishlisted) {
      removeFromWishlist(id);
    } else {
      addToWishlist(product);
    }
  };

  const handleQuickView = () => {
    // Implementar modal de vista rápida
    console.log('Quick view:', product);
  };

  return (
    <article 
      className={`product-card product-card--${variant}`}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      {/* Imagen del producto */}
      <div className="product-image-container">
        <Link to={`/product/${id}`} className="product-image-link">
          <img
            src={images[0]}
            alt={name}
            className={`product-image ${imageLoaded ? 'loaded' : ''}`}
            onLoad={() => setImageLoaded(true)}
            loading="lazy"
          />
          
          {/* Imagen secundaria en hover */}
          {images[1] && isHovered && (
            <img
              src={images[1]}
              alt={name}
              className="product-image product-image--secondary"
            />
          )}
        </Link>

        {/* Badges */}
        <div className="product-badges">
          {hasDiscount && (
            <span className="badge badge--discount">
              -{discountPercentage}%
            </span>
          )}
          
          {stock === 0 && (
            <span className="badge badge--out-of-stock">
              Agotado
            </span>
          )}
          
          {tags.includes('new') && (
            <span className="badge badge--new">
              Nuevo
            </span>
          )}
        </div>

        {/* Acciones rápidas */}
        <div className={`product-actions ${isHovered ? 'visible' : ''}`}>
          <button
            className="action-btn action-btn--wishlist"
            onClick={handleWishlistToggle}
            aria-label={isWishlisted ? 'Remover de favoritos' : 'Agregar a favoritos'}
          >
            <span className={`action-icon ${isWishlisted ? 'filled' : ''}`}>
              {isWishlisted ? '❤️' : '🤍'}
            </span>
          </button>
          
          <button
            className="action-btn action-btn--quick-view"
            onClick={handleQuickView}
            aria-label="Vista rápida"
          >
            <span className="action-icon">👁️</span>
          </button>
        </div>
      </div>

      {/* Información del producto */}
      <div className="product-content">
        {/* Categoría */}
        <div className="product-category">
          <Link to={`/category/${category.slug}`} className="category-link">
            {category.name}
          </Link>
        </div>

        {/* Nombre del producto */}
        <h3 className="product-name">
          <Link to={`/product/${id}`} className="product-name-link">
            {name}
          </Link>
        </h3>

        {/* Rating */}
        <div className="product-rating">
          <div className="rating-stars">
            {[...Array(5)].map((_, index) => (
              <span
                key={index}
                className={`star ${index < Math.floor(rating) ? 'filled' : ''}`}
              >
                {index < Math.floor(rating) ? '⭐' : '☆'}
              </span>
            ))}
          </div>
          <span className="rating-count">({reviewCount})</span>
        </div>

        {/* Precio */}
        <div className="product-price">
          {hasDiscount && (
            <span className="original-price">
              {formatPrice(originalPrice)}
            </span>
          )}
          <span className="current-price">
            {formatPrice(price)}
          </span>
        </div>

        {/* Stock */}
        {stock > 0 && stock <= 5 && (
          <div className="product-stock">
            <span className="stock-warning">
              ¡Solo quedan {stock} unidades!
            </span>
          </div>
        )}

        {/* Acciones */}
        <div className="product-actions-bottom">
          {stock > 0 ? (
            <>
              {cartQuantity > 0 ? (
                <div className="quantity-controls">
                  <button
                    className="quantity-btn"
                    onClick={() => addToCart(product, -1)}
                    disabled={cartQuantity <= 1}
                  >
                    -
                  </button>
                  <span className="quantity-display">{cartQuantity}</span>
                  <button
                    className="quantity-btn"
                    onClick={() => addToCart(product, 1)}
                    disabled={cartQuantity >= stock}
                  >
                    +
                  </button>
                </div>
              ) : (
                <button
                  className="btn btn-primary btn--add-to-cart"
                  onClick={handleAddToCart}
                >
                  <span className="btn-icon">🛒</span>
                  Agregar al Carrito
                </button>
              )}
            </>
          ) : (
            <button
              className="btn btn-secondary btn--notify-stock"
              disabled
            >
              Notificar Disponibilidad
            </button>
          )}
        </div>
      </div>
    </article>
  );
};

export default ProductCard;
```

### **2. CartDrawer.jsx (Carrito Lateral)**
```jsx
import React, { useState, useEffect } from 'react';
import { useCart } from '../../hooks/useCart';
import { useNavigate } from 'react-router-dom';
import { formatPrice, calculateTotal } from '../../utils/helpers';
import CartItem from './CartItem';
import './CartDrawer.css';

const CartDrawer = ({ isOpen, onClose }) => {
  const [isAnimating, setIsAnimating] = useState(false);
  const { cart, clearCart, getCartTotal } = useCart();
  const navigate = useNavigate();

  // Animación de entrada/salida
  useEffect(() => {
    if (isOpen) {
      setIsAnimating(true);
      document.body.style.overflow = 'hidden';
    } else {
      setIsAnimating(false);
      document.body.style.overflow = 'unset';
    }

    return () => {
      document.body.style.overflow = 'unset';
    };
  }, [isOpen]);

  const handleCheckout = () => {
    onClose();
    navigate('/checkout');
  };

  const handleContinueShopping = () => {
    onClose();
    navigate('/products');
  };

  const cartTotal = getCartTotal();
  const itemCount = cart.reduce((sum, item) => sum + item.quantity, 0);

  return (
    <>
      {/* Overlay */}
      {isOpen && (
        <div 
          className="cart-overlay"
          onClick={onClose}
        />
      )}

      {/* Drawer */}
      <div className={`cart-drawer ${isOpen ? 'open' : ''} ${isAnimating ? 'animating' : ''}`}>
        {/* Header */}
        <div className="cart-header">
          <h2 className="cart-title">
            Carrito de Compras
            {itemCount > 0 && (
              <span className="cart-count">({itemCount})</span>
            )}
          </h2>
          
          <button
            className="cart-close"
            onClick={onClose}
            aria-label="Cerrar carrito"
          >
            ✕
          </button>
        </div>

        {/* Contenido del carrito */}
        <div className="cart-content">
          {cart.length === 0 ? (
            <div className="cart-empty">
              <div className="empty-icon">🛒</div>
              <h3>Tu carrito está vacío</h3>
              <p>Agrega algunos productos para comenzar</p>
              <button
                className="btn btn-primary"
                onClick={handleContinueShopping}
              >
                Continuar Comprando
              </button>
            </div>
          ) : (
            <>
              {/* Lista de items */}
              <div className="cart-items">
                {cart.map((item) => (
                  <CartItem key={item.id} item={item} />
                ))}
              </div>

              {/* Resumen del carrito */}
              <div className="cart-summary">
                <div className="summary-row">
                  <span>Subtotal:</span>
                  <span>{formatPrice(cartTotal.subtotal)}</span>
                </div>
                
                {cartTotal.discount > 0 && (
                  <div className="summary-row summary-row--discount">
                    <span>Descuento:</span>
                    <span>-{formatPrice(cartTotal.discount)}</span>
                  </div>
                )}
                
                <div className="summary-row summary-row--total">
                  <span>Total:</span>
                  <span>{formatPrice(cartTotal.total)}</span>
                </div>

                {/* Cupón de descuento */}
                <div className="coupon-section">
                  <input
                    type="text"
                    placeholder="Código de descuento"
                    className="coupon-input"
                  />
                  <button className="btn btn-secondary btn--apply-coupon">
                    Aplicar
                  </button>
                </div>
              </div>

              {/* Acciones del carrito */}
              <div className="cart-actions">
                <button
                  className="btn btn-secondary btn--clear-cart"
                  onClick={clearCart}
                >
                  Limpiar Carrito
                </button>
                
                <button
                  className="btn btn-primary btn--checkout"
                  onClick={handleCheckout}
                >
                  Proceder al Checkout
                </button>
              </div>
            </>
          )}
        </div>

        {/* Footer con enlaces útiles */}
        <div className="cart-footer">
          <div className="cart-links">
            <a href="/help" className="cart-link">Ayuda</a>
            <a href="/shipping" className="cart-link">Envío</a>
            <a href="/returns" className="cart-link">Devoluciones</a>
          </div>
        </div>
      </div>
    </>
  );
};

export default CartDrawer;
```

### **3. CheckoutForm.jsx (Formulario de Checkout)**
```jsx
import React, { useState } from 'react';
import { useForm } from '../../hooks/useForm';
import { checkoutValidationRules } from '../../utils/validators';
import { useCart } from '../../hooks/useCart';
import { useCheckout } from '../../hooks/useCheckout';
import { formatPrice } from '../../utils/helpers';
import './CheckoutForm.css';

const CheckoutForm = () => {
  const [currentStep, setCurrentStep] = useState(1);
  const [paymentMethod, setPaymentMethod] = useState('credit');
  const [acceptTerms, setAcceptTerms] = useState(false);
  
  const { cart, getCartTotal } = useCart();
  const { processOrder, loading, error } = useCheckout();

  const {
    values,
    errors,
    touched,
    setValue,
    setFieldTouched,
    validateForm,
    resetForm
  } = useForm(
    {
      // Información personal
      firstName: '',
      lastName: '',
      email: '',
      phone: '',
      
      // Dirección de envío
      address: '',
      city: '',
      state: '',
      zipCode: '',
      country: '',
      
      // Información de pago
      cardNumber: '',
      cardName: '',
      expiryMonth: '',
      expiryYear: '',
      cvv: '',
      
      // Dirección de facturación
      billingSameAsShipping: true,
      billingAddress: '',
      billingCity: '',
      billingState: '',
      billingZipCode: '',
      billingCountry: ''
    },
    checkoutValidationRules
  );

  const cartTotal = getCartTotal();
  const totalSteps = 3;

  const steps = [
    { id: 1, title: 'Información Personal', icon: '👤' },
    { id: 2, title: 'Dirección de Envío', icon: '📍' },
    { id: 3, title: 'Pago', icon: '💳' }
  ];

  const handleNextStep = async () => {
    if (currentStep < totalSteps) {
      const isValid = await validateCurrentStep();
      if (isValid) {
        setCurrentStep(currentStep + 1);
      }
    }
  };

  const handlePrevStep = () => {
    if (currentStep > 1) {
      setCurrentStep(currentStep - 1);
    }
  };

  const validateCurrentStep = async () => {
    const fieldsToValidate = getFieldsForStep(currentStep);
    let isValid = true;

    for (const field of fieldsToValidate) {
      const error = await validateField(field);
      if (error) {
        isValid = false;
      }
    }

    return isValid;
  };

  const getFieldsForStep = (step) => {
    switch (step) {
      case 1:
        return ['firstName', 'lastName', 'email', 'phone'];
      case 2:
        return ['address', 'city', 'state', 'zipCode', 'country'];
      case 3:
        return ['cardNumber', 'cardName', 'expiryMonth', 'expiryYear', 'cvv'];
      default:
        return [];
    }
  };

  const validateField = async (fieldName) => {
    const error = checkoutValidationRules[fieldName](values[fieldName], values);
    if (error) {
      setFieldTouched(fieldName);
      return error;
    }
    return null;
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (!acceptTerms) {
      alert('Debes aceptar los términos y condiciones');
      return;
    }

    const isValid = await validateForm();
    if (isValid) {
      try {
        await processOrder({
          ...values,
          paymentMethod,
          items: cart,
          total: cartTotal.total
        });
        
        // Redirigir a confirmación
        navigate('/order-confirmation');
      } catch (error) {
        console.error('Error en checkout:', error);
      }
    }
  };

  const renderStepContent = () => {
    switch (currentStep) {
      case 1:
        return (
          <div className="checkout-step">
            <h3>Información Personal</h3>
            
            <div className="form-row">
              <div className="form-group">
                <label htmlFor="firstName">Nombre *</label>
                <input
                  id="firstName"
                  type="text"
                  value={values.firstName}
                  onChange={(e) => setValue('firstName', e.target.value)}
                  onBlur={() => setFieldTouched('firstName')}
                  className={touched.firstName && errors.firstName ? 'error' : ''}
                  required
                />
                {touched.firstName && errors.firstName && (
                  <span className="form-error">{errors.firstName}</span>
                )}
              </div>
              
              <div className="form-group">
                <label htmlFor="lastName">Apellido *</label>
                <input
                  id="lastName"
                  type="text"
                  value={values.lastName}
                  onChange={(e) => setValue('lastName', e.target.value)}
                  onBlur={() => setFieldTouched('lastName')}
                  className={touched.lastName && errors.lastName ? 'error' : ''}
                  required
                />
                {touched.lastName && errors.lastName && (
                  <span className="form-error">{errors.lastName}</span>
                )}
              </div>
            </div>

            <div className="form-row">
              <div className="form-group">
                <label htmlFor="email">Email *</label>
                <input
                  id="email"
                  type="email"
                  value={values.email}
                  onChange={(e) => setValue('email', e.target.value)}
                  onBlur={() => setFieldTouched('email')}
                  className={touched.email && errors.email ? 'error' : ''}
                  required
                />
                {touched.email && errors.email && (
                  <span className="form-error">{errors.email}</span>
                )}
              </div>
              
              <div className="form-group">
                <label htmlFor="phone">Teléfono *</label>
                <input
                  id="phone"
                  type="tel"
                  value={values.phone}
                  onChange={(e) => setValue('phone', e.target.value)}
                  onBlur={() => setFieldTouched('phone')}
                  className={touched.phone && errors.phone ? 'error' : ''}
                  required
                />
                {touched.phone && errors.phone && (
                  <span className="form-error">{errors.phone}</span>
                )}
              </div>
            </div>
          </div>
        );

      case 2:
        return (
          <div className="checkout-step">
            <h3>Dirección de Envío</h3>
            
            <div className="form-group">
              <label htmlFor="address">Dirección *</label>
              <input
                id="address"
                type="text"
                value={values.address}
                onChange={(e) => setValue('address', e.target.value)}
                onBlur={() => setFieldTouched('address')}
                className={touched.address && errors.address ? 'error' : ''}
                required
              />
              {touched.address && errors.address && (
                <span className="form-error">{errors.address}</span>
              )}
            </div>

            <div className="form-row">
              <div className="form-group">
                <label htmlFor="city">Ciudad *</label>
                <input
                  id="city"
                  type="text"
                  value={values.city}
                  onChange={(e) => setValue('city', e.target.value)}
                  onBlur={() => setFieldTouched('city')}
                  className={touched.city && errors.city ? 'error' : ''}
                  required
                />
                {touched.city && errors.city && (
                  <span className="form-error">{errors.city}</span>
                )}
              </div>
              
              <div className="form-group">
                <label htmlFor="state">Estado/Provincia *</label>
                <input
                  id="state"
                  type="text"
                  value={values.state}
                  onChange={(e) => setValue('state', e.target.value)}
                  onBlur={() => setFieldTouched('state')}
                  className={touched.state && errors.state ? 'error' : ''}
                  required
                />
                {touched.state && errors.state && (
                  <span className="form-error">{errors.state}</span>
                )}
              </div>
            </div>

            <div className="form-row">
              <div className="form-group">
                <label htmlFor="zipCode">Código Postal *</label>
                <input
                  id="zipCode"
                  type="text"
                  value={values.zipCode}
                  onChange={(e) => setValue('zipCode', e.target.value)}
                  onBlur={() => setFieldTouched('zipCode')}
                  className={touched.zipCode && errors.zipCode ? 'error' : ''}
                  required
                />
                {touched.zipCode && errors.zipCode && (
                  <span className="form-error">{errors.zipCode}</span>
                )}
              </div>
              
              <div className="form-group">
                <label htmlFor="country">País *</label>
                <select
                  id="country"
                  value={values.country}
                  onChange={(e) => setValue('country', e.target.value)}
                  onBlur={() => setFieldTouched('country')}
                  className={touched.country && errors.country ? 'error' : ''}
                  required
                >
                  <option value="">Seleccionar país</option>
                  <option value="ES">España</option>
                  <option value="MX">México</option>
                  <option value="AR">Argentina</option>
                  <option value="CO">Colombia</option>
                  <option value="PE">Perú</option>
                </select>
                {touched.country && errors.country && (
                  <span className="form-error">{errors.country}</span>
                )}
              </div>
            </div>
          </div>
        );

      case 3:
        return (
          <div className="checkout-step">
            <h3>Información de Pago</h3>
            
            <div className="payment-methods">
              <label className="payment-method">
                <input
                  type="radio"
                  name="paymentMethod"
                  value="credit"
                  checked={paymentMethod === 'credit'}
                  onChange={(e) => setPaymentMethod(e.target.value)}
                />
                <span className="payment-label">
                  💳 Tarjeta de Crédito/Débito
                </span>
              </label>
              
              <label className="payment-method">
                <input
                  type="radio"
                  name="paymentMethod"
                  value="paypal"
                  checked={paymentMethod === 'paypal'}
                  onChange={(e) => setPaymentMethod(e.target.value)}
                />
                <span className="payment-label">
                  🅿️ PayPal
                </span>
              </label>
            </div>

            {paymentMethod === 'credit' && (
              <div className="credit-card-form">
                <div className="form-group">
                  <label htmlFor="cardNumber">Número de Tarjeta *</label>
                  <input
                    id="cardNumber"
                    type="text"
                    value={values.cardNumber}
                    onChange={(e) => setValue('cardNumber', e.target.value)}
                    onBlur={() => setFieldTouched('cardNumber')}
                    className={touched.cardNumber && errors.cardNumber ? 'error' : ''}
                    placeholder="1234 5678 9012 3456"
                    maxLength="19"
                    required
                  />
                  {touched.cardNumber && errors.cardNumber && (
                    <span className="form-error">{errors.cardNumber}</span>
                  )}
                </div>

                <div className="form-group">
                  <label htmlFor="cardName">Nombre en la Tarjeta *</label>
                  <input
                    id="cardName"
                    type="text"
                    value={values.cardName}
                    onChange={(e) => setValue('cardName', e.target.value)}
                    onBlur={() => setFieldTouched('cardName')}
                    className={touched.cardName && errors.cardName ? 'error' : ''}
                    required
                  />
                  {touched.cardName && errors.cardName && (
                    <span className="form-error">{errors.cardName}</span>
                  )}
                </div>

                <div className="form-row">
                  <div className="form-group">
                    <label htmlFor="expiryMonth">Mes *</label>
                    <select
                      id="expiryMonth"
                      value={values.expiryMonth}
                      onChange={(e) => setValue('expiryMonth', e.target.value)}
                      onBlur={() => setFieldTouched('expiryMonth')}
                      className={touched.expiryMonth && errors.expiryMonth ? 'error' : ''}
                      required
                    >
                      <option value="">MM</option>
                      {[...Array(12)].map((_, i) => (
                        <option key={i + 1} value={String(i + 1).padStart(2, '0')}>
                          {String(i + 1).padStart(2, '0')}
                        </option>
                      ))}
                    </select>
                    {touched.expiryMonth && errors.expiryMonth && (
                      <span className="form-error">{errors.expiryMonth}</span>
                    )}
                  </div>
                  
                  <div className="form-group">
                    <label htmlFor="expiryYear">Año *</label>
                    <select
                      id="expiryYear"
                      value={values.expiryYear}
                      onChange={(e) => setValue('expiryYear', e.target.value)}
                      onBlur={() => setFieldTouched('expiryYear')}
                      className={touched.expiryYear && errors.expiryYear ? 'error' : ''}
                      required
                    >
                      <option value="">YYYY</option>
                      {[...Array(10)].map((_, i) => {
                        const year = new Date().getFullYear() + i;
                        return (
                          <option key={year} value={year}>
                            {year}
                          </option>
                        );
                      })}
                    </select>
                    {touched.expiryYear && errors.expiryYear && (
                      <span className="form-error">{errors.expiryYear}</span>
                    )}
                  </div>
                  
                  <div className="form-group">
                    <label htmlFor="cvv">CVV *</label>
                    <input
                      id="cvv"
                      type="text"
                      value={values.cvv}
                      onChange={(e) => setValue('cvv', e.target.value)}
                      onBlur={() => setFieldTouched('cvv')}
                      className={touched.cvv && errors.cvv ? 'error' : ''}
                      placeholder="123"
                      maxLength="4"
                      required
                    />
                    {touched.cvv && errors.cvv && (
                      <span className="form-error">{errors.cvv}</span>
                    )}
                  </div>
                </div>
              </div>
            )}

            {paymentMethod === 'paypal' && (
              <div className="paypal-info">
                <p>Serás redirigido a PayPal para completar tu pago de forma segura.</p>
                <div className="paypal-logo">🅿️ PayPal</div>
              </div>
            )}
          </div>
        );

      default:
        return null;
    }
  };

  return (
    <div className="checkout-container">
      {/* Pasos del checkout */}
      <div className="checkout-steps">
        {steps.map((step) => (
          <div
            key={step.id}
            className={`checkout-step-indicator ${
              step.id === currentStep ? 'active' : ''
            } ${step.id < currentStep ? 'completed' : ''}`}
          >
            <div className="step-icon">{step.icon}</div>
            <span className="step-title">{step.title}</span>
          </div>
        ))}
      </div>

      {/* Formulario */}
      <form onSubmit={handleSubmit} className="checkout-form">
        {renderStepContent()}

        {/* Términos y condiciones */}
        <div className="terms-section">
          <label className="terms-checkbox">
            <input
              type="checkbox"
              checked={acceptTerms}
              onChange={(e) => setAcceptTerms(e.target.checked)}
              required
            />
            <span className="terms-text">
              Acepto los{' '}
              <a href="/terms" target="_blank" rel="noopener noreferrer">
                términos y condiciones
              </a>{' '}
              y la{' '}
              <a href="/privacy" target="_blank" rel="noopener noreferrer">
                política de privacidad
              </a>
            </span>
          </label>
        </div>

        {/* Navegación entre pasos */}
        <div className="checkout-navigation">
          {currentStep > 1 && (
            <button
              type="button"
              className="btn btn-secondary"
              onClick={handlePrevStep}
            >
              ← Anterior
            </button>
          )}
          
          {currentStep < totalSteps ? (
            <button
              type="button"
              className="btn btn-primary"
              onClick={handleNextStep}
            >
              Siguiente →
            </button>
          ) : (
            <button
              type="submit"
              className="btn btn-primary btn--complete-order"
              disabled={loading || !acceptTerms}
            >
              {loading ? 'Procesando...' : `Completar Pedido - ${formatPrice(cartTotal.total)}`}
            </button>
          )}
        </div>

        {/* Resumen del pedido */}
        <div className="order-summary">
          <h4>Resumen del Pedido</h4>
          <div className="summary-items">
            {cart.map((item) => (
              <div key={item.id} className="summary-item">
                <span>{item.name} x {item.quantity}</span>
                <span>{formatPrice(item.price * item.quantity)}</span>
              </div>
            ))}
          </div>
          <div className="summary-total">
            <strong>Total: {formatPrice(cartTotal.total)}</strong>
          </div>
        </div>
      </form>
    </div>
  );
};

export default CheckoutForm;
```

---

## 🎨 **DISEÑO Y UX**

### **Sistema de Colores E-commerce**
```css
:root {
  /* Colores principales */
  --primary-color: #2563eb;
  --primary-hover: #1d4ed8;
  --secondary-color: #64748b;
  --accent-color: #f59e0b;
  
  /* Colores de estado */
  --success-color: #10b981;
  --warning-color: #f59e0b;
  --error-color: #ef4444;
  --info-color: #3b82f6;
  
  /* Colores de producto */
  --price-color: #059669;
  --discount-color: #dc2626;
  --stock-warning: #f97316;
  
  /* Colores de fondo */
  --bg-primary: #ffffff;
  --bg-secondary: #f8fafc;
  --bg-tertiary: #f1f5f9;
  --bg-dark: #0f172a;
  
  /* Colores de texto */
  --text-primary: #0f172a;
  --text-secondary: #475569;
  --text-muted: #64748b;
  --text-light: #ffffff;
  
  /* Sombras */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
}
```

---

## 📊 **CRITERIOS DE EVALUACIÓN**

### **Funcionalidad E-commerce (35%)**
- Catálogo de productos
- Carrito de compras
- Proceso de checkout
- Sistema de usuarios

### **Experiencia de Usuario (25%)**
- Navegación intuitiva
- Diseño responsive
- Performance optimizada
- Accesibilidad

### **Componentes React (25%)**
- Arquitectura modular
- Estado y props
- Hooks personalizados
- Manejo de eventos

### **Calidad del Código (15%)**
- Estructura organizada
- Manejo de errores
- Testing
- Documentación

---

## 💡 **CONSEJOS PARA LA IMPLEMENTACIÓN**

1. **Mobile First**: Desarrolla para móvil primero
2. **Optimiza conversión**: Flujo de checkout simple
3. **Performance**: Lazy loading y optimización
4. **Seguridad**: Validación de formularios
5. **Testing**: Prueba en diferentes dispositivos

---

*Este proyecto te permitirá crear una tienda online completa y profesional, demostrando dominio del frontend moderno.* 