import React, { useState } from 'react';
import { 
  ShoppingCart, Search, ShieldCheck, Download, Zap, 
  CreditCard, ExternalLink, HelpCircle, ArrowRight, Check, X, Menu, Star, Gamepad2, Laptop, Monitor
} from 'lucide-react';

export default function DigitalGameStore() {
  const [cart, setCart] = useState([]);
  const [isCartOpen, setIsCartOpen] = useState(false);
  const [selectedCategory, setSelectedCategory] = useState('Todos');
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedGame, setSelectedGame] = useState(null);
  const [isCheckoutOpen, setIsCheckoutOpen] = useState(false);
  const [checkoutStep, setCheckoutStep] = useState(1);
  const [customerEmail, setCustomerEmail] = useState('');
  const [paymentMethod, setPaymentMethod] = useState('mercadopago');
  const [promoCode, setPromoCode] = useState('');
  const [discount, setDiscount] = useState(0);
  const [orderComplete, setOrderComplete] = useState(false);
  const [purchasedKeys, setPurchasedKeys] = useState([]);

  // Catálogo de Juegos con precios en Pesos Argentinos (ARS)
  const games = [
    {
      id: 1,
      title: "Cyberpunk 2077: Ultimate Edition",
      category: "RPG",
      price: 28999,
      originalPrice: 45000,
      rating: 4.8,
      reviews: 1240,
      image: "https://images.unsplash.com/photo-1542751371-adc38448a05e?auto=format&fit=crop&w=800&q=80",
      description: "Sumérgete en Night City, un mundo abierto obsesionado con el poder, el glamour y la modificación corporal. Incluye la expansión Phantom Liberty.",
      platform: "Steam Key",
      instantDelivery: true,
      badge: "Más Vendido"
    },
    {
      id: 2,
      title: "EA SPORTS FC 26",
      category: "Deportes",
      price: 34999,
      originalPrice: 49999,
      rating: 4.5,
      reviews: 890,
      image: "https://images.unsplash.com/photo-1511512578047-dfb367046420?auto=format&fit=crop&w=800&q=80",
      description: "La experiencia futbolística más realista con HyperMotionV y plantillas actualizadas para la temporada 2026.",
      platform: "EA App Key",
      instantDelivery: true,
      badge: "Nuevo"
    },
    {
      id: 3,
      title: "Grand V Edition - Premium Online",
      category: "Acción",
      price: 14500,
      originalPrice: 29000,
      rating: 4.9,
      reviews: 3420,
      image: "https://images.unsplash.com/photo-1538481199705-c710c4e965fc?auto=format&fit=crop&w=800&q=80",
      description: "Explora el mundo de Los Santos y el condado de Blaine en la experiencia definitiva de mundo abierto.",
      platform: "Rockstar Key",
      instantDelivery: true,
      badge: "Oferta"
    },
    {
      id: 4,
      title: "Elden Ring: Shadow of the Erdtree",
      category: "RPG",
      price: 24999,
      originalPrice: 32000,
      rating: 4.9,
      reviews: 2150,
      image: "https://images.unsplash.com/photo-1550745165-9bc0b252726f?auto=format&fit=crop&w=800&q=80",
      description: "Guía tus pasos por el Reino de la Sombra en esta galardonada expansión que reta tus límites.",
      platform: "Steam Key",
      instantDelivery: true,
      badge: "Destacado"
    },
    {
      id: 5,
      title: "Minecraft: Java & Bedrock Edition",
      category: "Aventura",
      price: 12999,
      originalPrice: 18000,
      rating: 4.9,
      reviews: 5120,
      image: "https://images.unsplash.com/photo-1627856013091-fed6e4e3002ee?auto=format&fit=crop&w=800&q=80",
      description: "Juega con amigos en multiplataforma, sobrevive, construye y explora mundos infinitos.",
      platform: "Microsoft Key",
      instantDelivery: true,
      badge: "Popular"
    },
    {
      id: 6,
      title: "Silent Hill 2 Remake",
      category: "Terror",
      price: 31500,
      originalPrice: 42000,
      rating: 4.7,
      reviews: 630,
      image: "https://images.unsplash.com/photo-1509198397868-475647b2a1e5?auto=format&fit=crop&w=800&q=80",
      description: "Un clásico del terror psicológico reconstruido con gráficos de última generación.",
      platform: "Steam Key",
      instantDelivery: true,
      badge: "Terror"
    }
  ];

  const categories = ["Todos", "Acción", "RPG", "Deportes", "Aventura", "Terror"];

  // Funciones del Carrito
  const addToCart = (game) => {
    if (!cart.some(item => item.id === game.id)) {
      setCart([...cart, { ...game, quantity: 1 }]);
    }
    setIsCartOpen(true);
  };

  const removeFromCart = (id) => {
    setCart(cart.filter(item => item.id !== id));
  };

  const subtotal = cart.reduce((acc, item) => acc + item.price, 0);
  const total = Math.round(subtotal * (1 - discount));

  const applyPromoCode = () => {
    if (promoCode.toUpperCase() === 'GAMER10') {
      setDiscount(0.10);
      alert('¡Código aplicado con éxito! 10% de descuento.');
    } else {
      alert('Código inválido. Prueba con "GAMER10"');
    }
  };

  const handleCheckout = (e) => {
    e.preventDefault();
    if (!customerEmail) {
      alert('Por favor ingresa tu correo electrónico para recibir las claves.');
      return;
    }
    
    // Generar llaves digitales simuladas
    const keys = cart.map(item => ({
      gameTitle: item.title,
      platform: item.platform,
      key: `EVIL-${Math.random().toString(36).substring(2, 6).toUpperCase()}-${Math.random().toString(36).substring(2, 6).toUpperCase()}-${Math.random().toString(36).substring(2, 6).toUpperCase()}`
    }));

    setPurchasedKeys(keys);
    setOrderComplete(true);
    setCheckoutStep(3);
  };

  const filteredGames = games.filter(game => {
    const matchesCategory = selectedCategory === 'Todos' || game.category === selectedCategory;
    const matchesSearch = game.title.toLowerCase().includes(searchQuery.toLowerCase());
    return matchesCategory && matchesSearch;
  });

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 font-sans selection:bg-emerald-500 selection:text-black">
      
      {/* Barra de Navegación */}
      <header className="sticky top-0 z-40 bg-slate-900/90 backdrop-blur-md border-b border-slate-800">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
          <div className="flex items-center space-x-3 cursor-pointer" onClick={() => { setOrderComplete(false); setSelectedGame(null); }}>
            <div className="w-10 h-10 rounded-xl bg-gradient-to-tr from-emerald-500 to-cyan-500 flex items-center justify-center shadow-lg shadow-emerald-500/20">
              <Gamepad2 className="w-6 h-6 text-slate-950" />
            </div>
            <div>
              <span className="text-xl font-black tracking-wider bg-gradient-to-r from-emerald-400 to-cyan-400 bg-clip-text text-transparent">
                EVIL GAMES
              </span>
              <span className="block text-xs text-slate-400 font-medium">Soporte: evilsantos56@gmail.com</span>
            </div>
          </div>

          <div className="hidden md:flex items-center space-x-6">
            <a href="#catalogo" className="text-sm font-semibold hover:text-emerald-400 transition">Catálogo</a>
            <a href="#garantia" className="text-sm font-semibold hover:text-emerald-400 transition">Garantía</a>
            <a href="#faq" className="text-sm font-semibold hover:text-emerald-400 transition">Preguntas Frecuentes</a>
          </div>

          <div className="flex items-center space-x-4">
            <button 
              onClick={() => setIsCartOpen(true)}
              className="relative p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-200 transition border border-slate-700 flex items-center space-x-2"
            >
              <ShoppingCart className="w-5 h-5 text-emerald-400" />
              <span className="text-sm font-bold">{cart.length}</span>
            </button>
          </div>
        </div>
      </header>

      {/* Hero Principal */}
      <section className="relative overflow-hidden py-16 lg:py-24 border-b border-slate-800 bg-gradient-to-b from-slate-900 to-slate-950">
        <div className="absolute inset-0 opacity-20 bg-[radial-gradient(#10b981_1px,transparent_1px)] [background-size:16px_16px]"></div>
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10 text-center">
          <div className="inline-flex items-center space-x-2 px-3 py-1 rounded-full bg-emerald-500/10 border border-emerald-500/30 text-emerald-400 text-xs font-semibold mb-6">
            <Zap className="w-3.5 h-3.5" />
            <span>Entrega Inmediata 24/7 vía Correo Electrónico</span>
          </div>
          <h1 className="text-4xl sm:text-6xl font-black tracking-tight mb-6">
            Tus Juegos Digitales al <span className="bg-gradient-to-r from-emerald-400 to-cyan-400 bg-clip-text text-transparent">Mejor Precio en ARS</span>
          </h1>
          <p className="max-w-2xl mx-auto text-slate-400 text-base sm:text-lg mb-8">
            Compra de forma segura con Mercado Pago y transferencia. Recibe tus Keys originales para Steam, Epic y EA al instante.
          </p>

          {/* Barra de Búsqueda */}
          <div className="max-w-xl mx-auto relative">
            <Search className="absolute left-4 top-3.5 w-5 h-5 text-slate-400" />
            <input 
              type="text"
              placeholder="Busca tu juego favorito (ej. Cyberpunk, FIFA, Minecraft)..."
              value={searchQuery}
              onChange={(e) => setSearchQuery(e.target.value)}
              className="w-full pl-12 pr-4 py-3.5 rounded-2xl bg-slate-900 border border-slate-700 text-slate-100 placeholder-slate-500 focus:outline-none focus:border-emerald-500 transition shadow-xl"
            />
          </div>
        </div>
      </section>

      {/* Catálogo de Juegos */}
      <main id="catalogo" className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-8 gap-4">
          <h2 className="text-2xl font-bold">Juegos Destacados</h2>
          
          {/* Filtros de Categoría */}
          <div className="flex flex-wrap gap-2">
            {categories.map((cat) => (
              <button
                key={cat}
                onClick={() => setSelectedCategory(cat)}
                className={`px-4 py-2 rounded-xl text-xs font-bold transition ${
                  selectedCategory === cat 
                    ? 'bg-emerald-500 text-slate-950 shadow-lg shadow-emerald-500/20' 
                    : 'bg-slate-900 text-slate-400 hover:bg-slate-800 border border-slate-800'
                }`}
              >
                {cat}
              </button>
            ))}
          </div>
        </div>

        {/* Grilla de Juegos */}
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
          {filteredGames.map((game) => (
            <div key={game.id} className="bg-slate-900 rounded-2xl border border-slate-800 overflow-hidden hover:border-slate-700 transition flex flex-col group">
              <div className="relative h-48 overflow-hidden cursor-pointer" onClick={() => setSelectedGame(game)}>
                <img 
                  src={game.image} 
                  alt={game.title} 
                  className="w-full h-full object-cover group-hover:scale-105 transition duration-500"
                />
                <span className="absolute top-3 left-3 bg-emerald-500 text-slate-950 text-xs font-bold px-2.5 py-1 rounded-lg">
                  {game.badge}
                </span>
                <span className="absolute bottom-3 right-3 bg-slate-950/80 backdrop-blur-md text-slate-200 text-xs font-semibold px-2.5 py-1 rounded-lg border border-slate-800">
                  {game.platform}
                </span>
              </div>

              <div className="p-5 flex-1 flex flex-col justify-between">
                <div>
                  <div className="flex items-center justify-between text-xs text-slate-400 mb-2">
                    <span className="text-emerald-400 font-semibold">{game.category}</span>
                    <div className="flex items-center space-x-1">
                      <Star className="w-3.5 h-3.5 text-amber-400 fill-amber-400" />
                      <span>{game.rating}</span>
                    </div>
                  </div>
                  <h3 className="text-lg font-bold mb-2 group-hover:text-emerald-400 transition cursor-pointer" onClick={() => setSelectedGame(game)}>
                    {game.title}
                  </h3>
                  <p className="text-sm text-slate-400 line-clamp-2 mb-4">{game.description}</p>
                </div>

                <div className="pt-4 border-t border-slate-800 flex items-center justify-between">
                  <div>
                    <span className="text-xs text-slate-500 line-through block">${game.originalPrice.toLocaleString()} ARS</span>
                    <span className="text-xl font-black text-emerald-400">${game.price.toLocaleString()} ARS</span>
                  </div>
                  <button 
                    onClick={() => addToCart(game)}
                    className="px-4 py-2.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold text-sm transition shadow-lg shadow-emerald-500/10 flex items-center space-x-2"
                  >
                    <ShoppingCart className="w-4 h-4" />
                    <span>Comprar</span>
                  </button>
                </div>
              </div>
            </div>
          ))}
        </div>
      </main>

      {/* Sección de Garantía */}
      <section id="garantia" className="border-t border-slate-800 bg-slate-900/50 py-16">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-1 md:grid-cols-3 gap-8 text-center">
          <div className="p-6 rounded-2xl bg-slate-900 border border-slate-800">
            <ShieldCheck className="w-10 h-10 text-emerald-400 mx-auto mb-4" />
            <h3 className="font-bold text-lg mb-2">100% Originales y Garantizadas</h3>
            <p className="text-sm text-slate-400">Keys oficiales adquiridas de distribuidores autorizados con garantía de por vida.</p>
          </div>
          <div className="p-6 rounded-2xl bg-slate-900 border border-slate-800">
            <Zap className="w-10 h-10 text-emerald-400 mx-auto mb-4" />
            <h3 className="font-bold text-lg mb-2">Entrega Inmediata Automática</h3>
            <p className="text-sm text-slate-400">Una vez confirmado el pago, verás tu código en pantalla y te llegará al correo.</p>
          </div>
          <div className="p-6 rounded-2xl bg-slate-900 border border-slate-800">
            <CreditCard className="w-10 h-10 text-emerald-400 mx-auto mb-4" />
            <h3 className="font-bold text-lg mb-2">Pagos Locales en Pesos</h3>
            <p className="text-sm text-slate-400">Paga de forma fácil y sin sorpresas con Mercado Pago o Transferencia CBU.</p>
          </div>
        </div>
      </section>

      {/* Preguntas Frecuentes (FAQ) */}
      <section id="faq" className="max-w-4xl mx-auto px-4 py-16 border-t border-slate-800">
        <h2 className="text-2xl font-bold text-center mb-8">Preguntas Frecuentes</h2>
        <div className="space-y-4">
          <div className="p-6 rounded-2xl bg-slate-900 border border-slate-800">
            <h3 className="font-bold mb-2">¿Cómo recibo mi juego después de pagar?</h3>
            <p className="text-sm text-slate-400">El sistema te entrega la clave de activación (Key) de manera inmediata en la pantalla de éxito y también te la enviamos a evilsantos56@gmail.com junto con las instrucciones.</p>
          </div>
          <div className="p-6 rounded-2xl bg-slate-900 border border-slate-800">
            <h3 className="font-bold mb-2">¿Qué métodos de pago aceptan?</h3>
            <p className="text-sm text-slate-400">Aceptamos Mercado Pago, tarjetas de débito/crédito locales y transferencias bancarias mediante CVU/CBU en pesos argentinos.</p>
          </div>
          <div className="p-6 rounded-2xl bg-slate-900 border border-slate-800">
            <h3 className="font-bold mb-2">¿Tienen garantía las keys?</h3>
            <p className="text-sm text-slate-400">Sí, todas nuestras licencias cuentan con garantía total de activación. Si tienes cualquier inconveniente, contáctanos directamente a nuestro soporte.</p>
          </div>
        </div>
      </section>

      {/* Pie de Página */}
      <footer className="border-t border-slate-800 bg-slate-950 py-12 text-center text-sm text-slate-500">
        <p>© 2026 Evil Games Store. Todos los derechos reservados.</p>
        <p className="mt-2 text-xs text-slate-600">Contacto y soporte oficial: evilsantos56@gmail.com</p>
      </footer>

      {/* Modal Deslizante del Carrito */}
      {isCartOpen && (
        <div className="fixed inset-0 z-50 flex justify-end bg-black/60 backdrop-blur-sm">
          <div className="w-full max-w-md bg-slate-900 border-l border-slate-800 p-6 flex flex-col justify-between h-full shadow-2xl">
            <div>
              <div className="flex items-center justify-between mb-6">
                <h3 className="text-lg font-bold flex items-center space-x-2">
                  <ShoppingCart className="w-5 h-5 text-emerald-400" />
                  <span>Tu Carrito de Compras</span>
                </h3>
                <button onClick={() => setIsCartOpen(false)} className="p-2 rounded-xl bg-slate-800 hover:bg-slate-700">
                  <X className="w-5 h-5" />
                </button>
              </div>

              {cart.length === 0 ? (
                <div className="text-center py-20 text-slate-500">
                  <ShoppingCart className="w-16 h-16 mx-auto mb-4 opacity-30" />
                  <p>Tu carrito está vacío</p>
                </div>
              ) : (
                <div className="space-y-4 max-h-[50vh] overflow-y-auto pr-2">
                  {cart.map((item) => (
                    <div key={item.id} className="flex items-center justify-between p-3 rounded-xl bg-slate-950 border border-slate-800">
                      <div className="flex items-center space-x-3">
                        <img src={item.image} alt={item.title} className="w-12 h-12 object-cover rounded-lg" />
                        <div>
                          <h4 className="font-semibold text-sm">{item.title}</h4>
                          <span className="text-xs text-emerald-400 font-bold">${item.price.toLocaleString()} ARS</span>
                        </div>
                      </div>
                      <button onClick={() => removeFromCart(item.id)} className="text-slate-500 hover:text-red-400 p-1">
                        <X className="w-4 h-4" />
                      </button>
                    </div>
                  ))}
                </div>
              )}
            </div>

            {cart.length > 0 && (
              <div className="border-t border-slate-800 pt-6">
                {/* Código de Promoción */}
                <div className="flex space-x-2 mb-4">
                  <input 
                    type="text" 
                    placeholder="Código (ej. GAMER10)" 
                    value={promoCode}
                    onChange={(e) => setPromoCode(e.target.value)}
                    className="flex-1 bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-xs uppercase"
                  />
                  <button onClick={applyPromoCode} className="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-xs font-bold rounded-xl">
                    Aplicar
                  </button>
                </div>

                <div className="flex justify-between text-sm mb-2">
                  <span className="text-slate-400">Subtotal</span>
                  <span>${subtotal.toLocaleString()} ARS</span>
                </div>
                {discount > 0 && (
                  <div className="flex justify-between text-sm mb-2 text-emerald-400">
                    <span>Descuento (10%)</span>
                    <span>-${Math.round(subtotal * discount).toLocaleString()} ARS</span>
                  </div>
                )}
                <div className="flex justify-between text-lg font-bold mb-6">
                  <span>Total</span>
                  <span className="text-emerald-400">${total.toLocaleString()} ARS</span>
                </div>

                <button 
                  onClick={() => { setIsCartOpen(false); setIsCheckoutOpen(true); }}
                  className="w-full py-3.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold text-sm transition shadow-lg shadow-emerald-500/20 text-center block"
                >
                  Proceder al Pago
                </button>
              </div>
            )}
          </div>
        </div>
      )}

      {/* Modal de Checkout / Pago */}
      {isCheckoutOpen && (
        <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/70 backdrop-blur-md p-4">
          <div className="bg-slate-900 border border-slate-800 rounded-2xl max-w-lg w-full p-6 shadow-2xl">
            <div className="flex items-center justify-between mb-6">
              <h3 className="text-lg font-bold">Finalizar Compra</h3>
              <button onClick={() => { setIsCheckoutOpen(false); setOrderComplete(false); }} className="p-2 rounded-xl bg-slate-800 hover:bg-slate-700">
                <X className="w-5 h-5" />
              </button>
            </div>

            {!orderComplete ? (
              <form onSubmit={handleCheckout} className="space-y-4">
                <div>
                  <label className="block text-xs font-semibold text-slate-400 mb-1">Correo Electrónico (para recibir tus Keys)</label>
                  <input 
                    type="email" 
                    required
                    placeholder="tu-correo@gmail.com" 
                    value={customerEmail}
                    onChange={(e) => setCustomerEmail(e.target.value)}
                    className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm focus:border-emerald-500 focus:outline-none"
                  />
                </div>

                <div>
                  <label className="block text-xs font-semibold text-slate-400 mb-2">Método de Pago</label>
                  <div className="grid grid-cols-2 gap-3">
                    <button
                      type="button"
                      onClick={() => setPaymentMethod('mercadopago')}
                      className={`p-3 rounded-xl border text-sm font-semibold flex items-center justify-center space-x-2 ${
                        paymentMethod === 'mercadopago' ? 'border-emerald-500 bg-emerald-500/10 text-emerald-400' : 'border-slate-800 bg-slate-950 text-slate-400'
                      }`}
                    >
                      <span>Mercado Pago</span>
                    </button>
                    <button
                      type="button"
                      onClick={() => setPaymentMethod('transferencia')}
                      className={`p-3 rounded-xl border text-sm font-semibold flex items-center justify-center space-x-2 ${
                        paymentMethod === 'transferencia' ? 'border-emerald-500 bg-emerald-500/10 text-emerald-400' : 'border-slate-800 bg-slate-950 text-slate-400'
                      }`}
                    >
                      <span>Transferencia CBU</span>
                    </button>
                  </div>
                </div>

                <div className="p-4 rounded-xl bg-slate-950 border border-slate-800 text-xs text-slate-400 space-y-1">
                  <p>• Total a pagar: <strong className="text-emerald-400">${total.toLocaleString()} ARS</strong></p>
                  <p>• Soporte de asistencia directa en evilsantos56@gmail.com</p>
                </div>

                <button 
                  type="submit"
                  className="w-full py-3.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold text-sm transition shadow-lg shadow-emerald-500/20"
                >
                  Confirmar Pago y Obtener Claves
                </button>
              </form>
            ) : (
              <div className="text-center space-y-4">
                <div className="w-16 h-16 bg-emerald-500/10 border border-emerald-500/30 rounded-full flex items-center justify-center mx-auto text-emerald-400">
                  <Check className="w-8 h-8" />
                </div>
                <h4 className="text-xl font-bold">¡Compra Exitosa!</h4>
                <p className="text-sm text-slate-400">Tus claves digitales han sido generadas y enviadas a <strong className="text-slate-200">{customerEmail}</strong>.</p>
                
                <div className="space-y-3 text-left max-h-60 overflow-y-auto my-4 pr-1">
                  {purchasedKeys.map((item, idx) => (
                    <div key={idx} className="p-3 rounded-xl bg-slate-950 border border-slate-800">
                      <span className="text-xs font-bold text-emerald-400 block">{item.gameTitle} ({item.platform})</span>
                      <div className="flex items-center justify-between mt-1">
                        <code className="text-xs bg-slate-900 px-2 py-1 rounded text-slate-200 select-all">{item.key}</code>
                        <button 
                          onClick={() => { navigator.clipboard.writeText(item.key); alert('¡Clave copiada al portapapeles!'); }}
                          className="text-xs bg-emerald-500/20 hover:bg-emerald-500/30 text-emerald-400 px-2 py-1 rounded font-semibold"
                        >
                          Copiar
                        </button>
                      </div>
                    </div>
                  ))}
                </div>

                <button 
                  onClick={() => { setIsCheckoutOpen(false); setCart([]); setOrderComplete(false); }}
                  className="w-full py-3 rounded-xl bg-slate-800 hover:bg-slate-700 text-sm font-bold"
                >
                  Volver a la Tienda
                </button>
              </div>
            )}
          </div>
        </div>
      )}

      {/* Modal de Detalle de Juego */}
      {selectedGame && (
        <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/70 backdrop-blur-md p-4">
          <div className="bg-slate-900 border border-slate-800 rounded-2xl max-w-2xl w-full overflow-hidden shadow-2xl relative">
            <button onClick={() => setSelectedGame(null)} className="absolute top-4 right-4 z-10 p-2 rounded-xl bg-slate-950/80 hover:bg-slate-900 text-slate-200">
              <X className="w-5 h-5" />
            </button>
            <div className="h-64 overflow-hidden relative">
              <img src={selectedGame.image} alt={selectedGame.title} className="w-full h-full object-cover" />
              <div className="absolute inset-0 bg-gradient-to-t from-slate-900 via-transparent"></div>
            </div>
            <div className="p-6">
              <div className="flex items-center justify-between mb-2">
                <span className="text-xs font-bold text-emerald-400 px-3 py-1 rounded-full bg-emerald-500/10 border border-emerald-500/20">{selectedGame.category}</span>
                <span className="text-xs text-slate-400">Plataforma: <strong className="text-slate-200">{selectedGame.platform}</strong></span>
              </div>
              <h3 className="text-2xl font-bold mb-3">{selectedGame.title}</h3>
              <p className="text-slate-400 text-sm mb-6">{selectedGame.description}</p>
              
              <div className="flex items-center justify-between pt-4 border-t border-slate-800">
                <div>
                  <span className="text-xs text-slate-500 line-through block">${selectedGame.originalPrice.toLocaleString()} ARS</span>
                  <span className="text-2xl font-black text-emerald-400">${selectedGame.price.toLocaleString()} ARS</span>
                </div>
                <button 
                  onClick={() => { addToCart(selectedGame); setSelectedGame(null); }}
                  className="px-6 py-3 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold text-sm transition shadow-lg shadow-emerald-500/20 flex items-center space-x-2"
                >
                  <ShoppingCart className="w-4 h-4" />
                  <span>Agregar al Carrito</span>
                </button>
              </div>
            </div>
          </div>
        </div>
      )}

    </div>
  );
}