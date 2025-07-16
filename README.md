# Information-Architecture-Wireframing
import React, { useState, useEffect } from 'react';

// Dummy Data for demonstration
const categories = [
  { id: 'all', name: 'All', icon: '✨', filter: 'all' }, // Added 'All' category
  { id: 'pizza', name: 'Pizza', icon: '🍕', filter: 'Italian' }, // Filter by cuisine
  { id: 'sushi', name: 'Sushi', icon: '�', filter: 'Japanese' },
  { id: 'burgers', name: 'Burgers', icon: '🍔', filter: 'American' },
  { id: 'desserts', name: 'Desserts', icon: '🍰', filter: 'Dessert' }, // Example filter
  { id: 'drinks', name: 'Drinks', icon: '🥤', filter: 'Drinks' }, // Example filter
  { id: 'indian', name: 'Indian', icon: '🍛', filter: 'Indian' }, // New: Indian category
];

const restaurants = [
  {
    id: 'rest1',
    name: 'Pizza Paradise',
    cuisine: 'Italian',
    rating: 4.5,
    deliveryTime: '30-45 min',
    deliveryFee: '$2.99',
    imageUrl: 'https://placehold.co/400x200/FF5733/FFFFFF?text=Pizza+Paradise',
    menu: [
      { id: 'm1', name: 'Pepperoni Pizza', price: 15.99, description: 'Classic pepperoni pizza' },
      { id: 'm2', name: 'Margherita Pizza', price: 13.50, description: 'Tomato, mozzarella, basil' },
      { id: 'm3', name: 'Garlic Bread', price: 5.00, description: 'Cheesy garlic bread' },
    ],
  },
  {
    id: 'rest2',
    name: 'Sushi Spot',
    cuisine: 'Japanese',
    rating: 4.8,
    deliveryTime: '20-30 min',
    deliveryFee: '$1.50',
    imageUrl: 'https://placehold.co/400x200/3366FF/FFFFFF?text=Sushi+Spot',
    menu: [
      { id: 'm4', name: 'California Roll', price: 12.00, description: 'Crab, avocado, cucumber' },
      { id: 'm5', name: 'Spicy Tuna Roll', price: 14.50, description: 'Tuna, spicy mayo' },
      { id: 'm6', name: 'Miso Soup', price: 4.00, description: 'Traditional miso soup' },
    ],
  },
  {
    id: 'rest3',
    name: 'Burger Joint',
    cuisine: 'American',
    rating: 4.2,
    deliveryTime: '40-55 min',
    deliveryFee: '$3.50',
    imageUrl: 'https://placehold.co/400x200/33FF57/FFFFFF?text=Burger+Joint',
    menu: [
      { id: 'm7', name: 'Classic Burger', price: 10.00, description: 'Beef patty, lettuce, tomato' },
      { id: 'm8', name: 'Cheeseburger', price: 11.50, description: 'Classic with cheese' },
      { id: 'm9', name: 'Fries', price: 4.00, description: 'Crispy golden fries' },
    ],
  },
  // Adding a dessert restaurant for demonstration
  {
    id: 'rest4',
    name: 'Sweet Tooth Bakery',
    cuisine: 'Dessert',
    rating: 4.7,
    deliveryTime: '25-35 min',
    deliveryFee: '$2.00',
    imageUrl: 'https://placehold.co/400x200/F08080/FFFFFF?text=Sweet+Tooth+Bakery',
    menu: [
      { id: 'm10', name: 'Chocolate Cake', price: 8.00, description: 'Rich chocolate fudge cake' },
      { id: 'm11', name: 'Cheesecake Slice', price: 7.50, description: 'Creamy New York style cheesecake' },
    ],
  },
  // Adding a drinks restaurant for demonstration
  {
    id: 'rest5',
    name: 'The Drink Bar',
    cuisine: 'Drinks',
    rating: 4.6,
    deliveryTime: '15-25 min',
    deliveryFee: '$1.00',
    imageUrl: 'https://placehold.co/400x200/4CAF50/FFFFFF?text=The+Drink+Bar',
    menu: [
      { id: 'm12', name: 'Fresh Orange Juice', price: 6.00, description: 'Freshly squeezed orange juice' },
      { id: 'm13', name: 'Iced Coffee', price: 5.50, description: 'Chilled coffee with ice' },
      { id: 'm14', name: 'Sparkling Water', price: 3.00, description: 'Crisp sparkling water' },
    ],
  },
  // New: Adding a Biryani restaurant for demonstration
  {
    id: 'rest6',
    name: 'Biryani House',
    cuisine: 'Indian',
    rating: 4.9,
    deliveryTime: '35-50 min',
    deliveryFee: '$2.50',
    imageUrl: 'https://placehold.co/400x200/8B4513/FFFFFF?text=Biryani+House',
    menu: [
      { id: 'm15', name: 'Chicken Biryani', price: 16.99, description: 'Fragrant basmati rice cooked with tender chicken and spices' },
      { id: 'm16', name: 'Vegetable Biryani', price: 14.50, description: 'Aromatic rice dish with mixed vegetables and exotic spices' },
      { id: 'm17', name: 'Naan Bread', price: 3.00, description: 'Traditional Indian flatbread' },
    ],
  },
];

// Main App Component
const App = () => {
  const [currentView, setCurrentView] = useState('home'); // 'home', 'restaurant', 'cart'
  const [selectedRestaurant, setSelectedRestaurant] = useState(null);
  const [cart, setCart] = useState([]); // [{ item: {id, name, price}, quantity: N, restaurantId: 'restId' }]
  const [showCartMessage, setShowCartMessage] = useState(false);
  const [cartMessage, setCartMessage] = useState('');
  const [filteredRestaurants, setFilteredRestaurants] = useState(restaurants); // New state for filtered restaurants
  const [activeCategory, setActiveCategory] = useState('all'); // To highlight active category

  // Function to navigate between views
  const navigateTo = (view, data = null) => {
    setCurrentView(view);
    if (view === 'restaurant') {
      setSelectedRestaurant(data);
    } else {
      setSelectedRestaurant(null);
    }
    // Clear cart message on navigation
    setShowCartMessage(false);
    setCartMessage('');
  };

  // Function to filter restaurants by category
  const filterRestaurantsByCategory = (categoryFilter) => {
    setActiveCategory(categoryFilter); // Set active category for highlighting
    if (categoryFilter === 'all') {
      setFilteredRestaurants(restaurants);
    } else {
      const filtered = restaurants.filter(
        (restaurant) => restaurant.cuisine === categoryFilter
      );
      setFilteredRestaurants(filtered);
    }
  };

  // Add item to cart
  const addToCart = (item, restaurantId) => {
    // Check if the item is from the same restaurant as existing cart items
    if (cart.length > 0 && cart[0].restaurantId !== restaurantId) {
      setCartMessage('You can only order from one restaurant at a time. Clear your cart to order from a new restaurant.');
      setShowCartMessage(true);
      return;
    }

    const existingItemIndex = cart.findIndex(cartItem => cartItem.item.id === item.id);
    if (existingItemIndex > -1) {
      const newCart = [...cart];
      newCart[existingItemIndex].quantity += 1;
      setCart(newCart);
    } else {
      setCart([...cart, { item, quantity: 1, restaurantId }]);
    }
    setCartMessage(`${item.name} added to cart!`);
    setShowCartMessage(true);
  };

  // Remove item from cart
  const removeFromCart = (itemId) => {
    const existingItemIndex = cart.findIndex(cartItem => cartItem.item.id === itemId);
    if (existingItemIndex > -1) {
      const newCart = [...cart];
      if (newCart[existingItemIndex].quantity > 1) {
        newCart[existingItemIndex].quantity -= 1;
      } else {
        newCart.splice(existingItemIndex, 1);
      }
      setCart(newCart);
      setCartMessage('Item quantity updated.');
      setShowCartMessage(true);
    }
  };

  // Clear the entire cart
  const clearCart = () => {
    setCart([]);
    setCartMessage('Cart cleared!');
    setShowCartMessage(true);
  };

  // Calculate total cart value
  const calculateTotal = () => {
    return cart.reduce((total, cartItem) => total + (cartItem.item.price * cartItem.quantity), 0).toFixed(2);
  };

  // Effect to hide cart message after a few seconds
  useEffect(() => {
    if (showCartMessage) {
      const timer = setTimeout(() => {
        setShowCartMessage(false);
        setCartMessage('');
      }, 3000); // Hide after 3 seconds
      return () => clearTimeout(timer);
    }
  }, [showCartMessage]);

  // Render different views based on currentView state
  const renderView = () => {
    switch (currentView) {
      case 'home':
        return (
          <div className="p-4 md:p-8">
            <h2 className="text-3xl font-bold text-gray-800 mb-6">Categories</h2>
            <div className="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4 mb-10">
              {categories.map(category => (
                <div
                  key={category.id}
                  className={`flex flex-col items-center justify-center p-4 rounded-lg shadow-sm hover:shadow-md transition-shadow cursor-pointer
                    ${activeCategory === category.filter ? 'bg-blue-200 border-2 border-blue-500' : 'bg-blue-50'}`}
                  onClick={() => filterRestaurantsByCategory(category.filter)}
                >
                  <span className="text-4xl mb-2">{category.icon}</span>
                  <p className="text-lg font-medium text-gray-700">{category.name}</p>
                </div>
              ))}
            </div>

            <h2 className="text-3xl font-bold text-gray-800 mb-6">Popular Restaurants</h2>
            {filteredRestaurants.length === 0 ? (
                <p className="text-gray-600 text-lg text-center bg-white p-6 rounded-lg shadow-md">No restaurants found for this category.</p>
            ) : (
                <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                {filteredRestaurants.map(restaurant => (
                    <div
                    key={restaurant.id}
                    className="bg-white rounded-lg shadow-md overflow-hidden cursor-pointer transform transition-transform hover:scale-105"
                    onClick={() => navigateTo('restaurant', restaurant)}
                    >
                    <img src={restaurant.imageUrl} alt={restaurant.name} className="w-full h-40 object-cover" />
                    <div className="p-4">
                        <h3 className="text-xl font-semibold text-gray-800 mb-1">{restaurant.name}</h3>
                        <p className="text-gray-600 text-sm mb-2">{restaurant.cuisine} • {restaurant.rating} ⭐</p>
                        <div className="flex justify-between items-center text-sm text-gray-500">
                        <span>{restaurant.deliveryTime}</span>
                        <span>{restaurant.deliveryFee} Delivery Fee</span>
                        </div>
                    </div>
                    </div>
                ))}
                </div>
            )}
          </div>
        );
      case 'restaurant':
        return (
          <div className="p-4 md:p-8">
            <button
              onClick={() => navigateTo('home')}
              className="flex items-center text-blue-600 hover:text-blue-800 mb-6 font-medium transition-colors"
            >
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
                <path fillRule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clipRule="evenodd" />
              </svg>
              Back to Restaurants
            </button>
            {selectedRestaurant && (
              <div className="bg-white rounded-lg shadow-xl overflow-hidden">
                <img src={selectedRestaurant.imageUrl} alt={selectedRestaurant.name} className="w-full h-60 object-cover" />
                <div className="p-6">
                  <h2 className="text-3xl font-bold text-gray-800 mb-2">{selectedRestaurant.name}</h2>
                  <p className="text-gray-600 text-lg mb-4">{selectedRestaurant.cuisine} • {selectedRestaurant.rating} ⭐</p>

                  <h3 className="text-2xl font-semibold text-gray-800 mb-4">Menu</h3>
                  <div className="space-y-4">
                    {selectedRestaurant.menu.map(item => (
                      <div key={item.id} className="flex justify-between items-center bg-gray-50 p-4 rounded-md shadow-sm">
                        <div>
                          <p className="text-xl font-medium text-gray-800">{item.name}</p>
                          <p className="text-gray-600 text-sm">{item.description}</p>
                        </div>
                        <div className="flex items-center">
                          <span className="text-lg font-semibold text-gray-800 mr-4">${item.price.toFixed(2)}</span>
                          <button
                            onClick={() => addToCart(item, selectedRestaurant.id)}
                            className="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full transition-colors"
                          >
                            Add
                          </button>
                        </div>
                      </div>
                    ))}
                  </div>
                </div>
              </div>
            )}
          </div>
        );
      case 'cart':
        return (
          <div className="p-4 md:p-8">
            <button
              onClick={() => navigateTo('home')}
              className="flex items-center text-blue-600 hover:text-blue-800 mb-6 font-medium transition-colors"
            >
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
                <path fillRule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clipRule="evenodd" />
              </svg>
              Back to Home
            </button>
            <h2 className="text-3xl font-bold text-gray-800 mb-6">Your Cart</h2>
            {cart.length === 0 ? (
              <p className="text-gray-600 text-lg text-center bg-white p-6 rounded-lg shadow-md">Your cart is empty.</p>
            ) : (
              <div className="bg-white rounded-lg shadow-xl p-6">
                <div className="space-y-4 mb-6">
                  {cart.map(cartItem => (
                    <div key={cartItem.item.id} className="flex justify-between items-center border-b pb-4 last:border-b-0">
                      <div>
                        <p className="text-xl font-medium text-gray-800">{cartItem.item.name}</p>
                        <p className="text-gray-600 text-sm">Quantity: {cartItem.quantity}</p>
                      </div>
                      <div className="flex items-center">
                        <span className="text-lg font-semibold text-gray-800 mr-4">${(cartItem.item.price * cartItem.quantity).toFixed(2)}</span>
                        <button
                          onClick={() => removeFromCart(cartItem.item.id)}
                          className="bg-red-500 hover:bg-red-600 text-white font-bold py-1 px-3 rounded-full text-sm transition-colors"
                        >
                          Remove
                        </button>
                      </div>
                    </div>
                  ))}
                </div>
                <div className="border-t pt-4 flex justify-between items-center">
                  <p className="text-2xl font-bold text-gray-800">Total:</p>
                  <p className="text-2xl font-bold text-blue-600">${calculateTotal()}</p>
                </div>
                <div className="mt-8 flex flex-col sm:flex-row justify-end gap-4">
                  <button
                    onClick={clearCart}
                    className="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-3 px-6 rounded-full shadow-md transition-colors"
                  >
                    Clear Cart
                  </button>
                  <button
                    className="bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-full shadow-lg transition-colors"
                  >
                    Proceed to Checkout
                  </button>
                </div>
              </div>
            )}
          </div>
        );
      default:
        return null;
    }
  };

  return (
    <div className="min-h-screen bg-gray-100 flex flex-col">
      {/* Top Navigation Bar */}
      <header className="bg-white shadow-md p-4 flex justify-between items-center sticky top-0 z-10">
        <h1 className="text-2xl font-bold text-gray-800">Food App Prototype</h1>
        <div className="flex items-center space-x-4">
          <input
            type="text"
            placeholder="Search for restaurants or dishes..."
            className="p-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 w-40 sm:w-64"
          />
          <button
            onClick={() => navigateTo('cart')}
            className="relative bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full transition-colors flex items-center"
          >
            <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
              <path strokeLinecap="round" strokeLinejoin="round" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.182 1.703.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
            </svg>
            {cart.length > 0 && (
              <span className="absolute -top-2 -right-2 bg-red-500 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center">
                {cart.reduce((sum, item) => sum + item.quantity, 0)}
              </span>
            )}
          </button>
        </div>
      </header>

      {/* Cart Message Display */}
      {showCartMessage && (
        <div className="fixed top-16 left-1/2 -translate-x-1/2 bg-gray-800 text-white px-6 py-3 rounded-full shadow-lg z-20 transition-opacity duration-300">
          {cartMessage}
        </div>
      )}

      <main className="flex-grow container mx-auto py-8">
        {renderView()}
      </main>

      {/* Bottom Navigation Bar */}
      <nav className="bg-white shadow-lg p-4 fixed bottom-0 left-0 right-0 z-10 md:hidden">
        <ul className="flex justify-around text-center text-gray-600">
          <li className="flex-1">
            <button onClick={() => navigateTo('home')} className="flex flex-col items-center w-full focus:outline-none">
              <svg xmlns="http://www.w3.org/2000/svg" className={`h-6 w-6 ${currentView === 'home' ? 'text-blue-600' : ''}`} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                <path strokeLinecap="round" strokeLinejoin="round" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
              </svg>
              <span className={`text-xs ${currentView === 'home' ? 'text-blue-600 font-semibold' : ''}`}>Home</span>
            </button>
          </li>
          <li className="flex-1">
            <button onClick={() => navigateTo('cart')} className="relative flex flex-col items-center w-full focus:outline-none">
              <svg xmlns="http://www.w3.org/2000/svg" className={`h-6 w-6 ${currentView === 'cart' ? 'text-blue-600' : ''}`} fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                <path strokeLinecap="round" strokeLinejoin="round" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.182 1.703.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
              </svg>
              {cart.length > 0 && (
                <span className="absolute top-0 right-1/4 bg-red-500 text-white text-xs font-bold rounded-full h-4 w-4 flex items-center justify-center">
                  {cart.reduce((sum, item) => sum + item.quantity, 0)}
                </span>
              )}
              <span className={`text-xs ${currentView === 'cart' ? 'text-blue-600 font-semibold' : ''}`}>Cart</span>
            </button>
          </li>
          <li className="flex-1">
            <button className="flex flex-col items-center w-full focus:outline-none">
              <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                <path strokeLinecap="round" strokeLinejoin="round" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
              </svg>
              <span className="text-xs">Account</span>
            </button>
          </li>
        </ul>
      </nav>
    </div>
  );
};

export default App;
�
