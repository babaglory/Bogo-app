import React, { useState } from 'react';
import { Camera, Share2, ShoppingCart, Users, Crown, Search, Bell, Filter, MapPin } from 'lucide-react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Alert, AlertDescription } from '@/components/ui/alert';

const BOGOApp = () => {
  const [activeTab, setActiveTab] = useState('deals');
  const [scannedItem, setScannedItem] = useState(null);
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedCategory, setSelectedCategory] = useState('all');
  
  const deals = [
    {
      id: 1,
      store: "Tesco",
      item: "Chocolate Bar",
      category: "Food",
      originalPrice: 2.00,
      bogoPrice: 1.00,
      seekingPartner: true,
      distance: "0.5 miles",
      expires: "2 days",
      image: "/api/placeholder/100/100"
    },
    {
      id: 2,
      store: "ASDA",
      item: "Pizza",
      category: "Food",
      originalPrice: 4.00,
      bogoPrice: 2.00,
      seekingPartner: false,
      distance: "1.2 miles",
      expires: "1 day",
      image: "/api/placeholder/100/100"
    },
    {
      id: 3,
      store: "Boots",
      item: "Shampoo",
      category: "Health",
      originalPrice: 6.00,
      bogoPrice: 3.00,
      seekingPartner: true,
      distance: "0.8 miles",
      expires: "5 days",
      image: "/api/placeholder/100/100"
    }
  ];

  const categories = ['all', 'Food', 'Health', 'Electronics', 'Fashion'];

  const filteredDeals = deals.filter(deal => 
    (selectedCategory === 'all' || deal.category === selectedCategory) &&
    (deal.item.toLowerCase().includes(searchQuery.toLowerCase()) ||
     deal.store.toLowerCase().includes(searchQuery.toLowerCase()))
  );

  const handleScan = () => {
    setScannedItem({
      store: "Local Store",
      item: "Scanned Item",
      originalPrice: 3.00,
      bogoPrice: 1.50,
      validUntil: new Date(Date.now() + 86400000).toLocaleDateString()
    });
  };

  return (
    <div className="max-w-md mx-auto p-4 space-y-4">
      <div className="flex items-center justify-between mb-4">
        <h1 className="text-xl font-bold">BOGO</h1>
        <Bell className="w-6 h-6" />
      </div>

      <div className="relative mb-4">
        <Search className="absolute left-3 top-2.5 w-5 h-5 text-gray-400" />
        <input
          type="text"
          placeholder="Search deals and stores..."
          className="w-full pl-10 pr-4 py-2 border rounded-lg"
          value={searchQuery}
          onChange={(e) => setSearchQuery(e.target.value)}
        />
      </div>

      <div className="flex gap-2 overflow-x-auto pb-2">
        {categories.map(category => (
          <button
            key={category}
            onClick={() => setSelectedCategory(category)}
            className={`px-3 py-1 rounded-full text-sm whitespace-nowrap 
              ${selectedCategory === category 
                ? 'bg-blue-500 text-white' 
                : 'bg-gray-100'}`}
          >
            {category}
          </button>
        ))}
      </div>

      <nav className="flex justify-around mb-6">
        <button 
          onClick={() => setActiveTab('deals')}
          className={`p-2 ${activeTab === 'deals' ? 'border-b-2 border-blue-500' : ''}`}
        >
          <ShoppingCart className="w-6 h-6" />
        </button>
        <button 
          onClick={() => setActiveTab('scan')}
          className={`p-2 ${activeTab === 'scan' ? 'border-b-2 border-blue-500' : ''}`}
        >
          <Camera className="w-6 h-6" />
        </button>
        <button 
          onClick={() => setActiveTab('social')}
          className={`p-2 ${activeTab === 'social' ? 'border-b-2 border-blue-500' : ''}`}
        >
          <Users className="w-6 h-6" />
        </button>
      </nav>

      {activeTab === 'deals' && (
        <div className="space-y-4">
          {filteredDeals.map(deal => (
            <Card key={deal.id}>
              <CardContent className="p-4">
                <div className="flex items-center space-x-4">
                  <img src={deal.image} alt={deal.item} className="w-20 h-20 rounded" />
                  <div className="flex-1">
                    <h3 className="font-bold">{deal.item}</h3>
                    <div className="flex items-center text-sm text-gray-600">
                      <MapPin className="w-4 h-4 mr-1" />
                      {deal.store} • {deal.distance}
                    </div>
                    <div className="flex justify-between items-center mt-2">
                      <div>
                        <p className="line-through text-gray-500">£{deal.originalPrice.toFixed(2)}</p>
                        <p className="font-bold text-green-600">£{deal.bogoPrice.toFixed(2)}</p>
                        <p className="text-xs text-gray-500">Expires in {deal.expires}</p>
                      </div>
                      {deal.seekingPartner && (
                        <button className="bg-blue-500 text-white px-3 py-1 rounded flex items-center">
                          <Users className="w-4 h-4 mr-1" />
                          Join
                        </button>
                      )}
                    </div>
                  </div>
                </div>
              </CardContent>
            </Card>
          ))}
        </div>
      )}

      {activeTab === 'scan' && (
        <div className="space-y-4">
          <Card>
            <CardHeader>
              <CardTitle className="flex items-center">
                <Crown className="w-5 h-5 text-yellow-500 mr-2" />
                BOGO Checkout
              </CardTitle>
            </CardHeader>
            <CardContent>
              <button 
                onClick={handleScan}
                className="w-full bg-blue-500 text-white p-4 rounded flex items-center justify-center"
              >
                <Camera className="w-6 h-6 mr-2" />
                Scan Item
              </button>
              
              {scannedItem && (
                <Alert className="mt-4">
                  <AlertDescription>
                    <div className="space-y-2">
                      <p className="font-bold">{scannedItem.item}</p>
                      <p>Original: £{scannedItem.originalPrice.toFixed(2)}</p>
                      <p className="text-green-600">BOGO Price: £{scannedItem.bogoPrice.toFixed(2)}</p>
                      <p className="text-sm text-gray-500">Valid until: {scannedItem.validUntil}</p>
                    </div>
                  </AlertDescription>
                </Alert>
              )}
            </CardContent>
          </Card>
        </div>
      )}

      {activeTab === 'social' && (
        <div className="space-y-4">
          <Card>
            <CardContent className="p-4">
              <div className="space-y-4">
                <div className="flex items-center justify-between">
                  <h3 className="font-bold">Share Your BOGO List</h3>
                  <Share2 className="w-5 h-5 text-blue-500" />
                </div>
                
                <div className="grid grid-cols-2 gap-2">
                  {['TikTok', 'Instagram', 'Twitter', 'Facebook'].map(platform => (
                    <button
                      key={platform}
                      className="bg-gray-100 px-4 py-2 rounded-lg text-sm flex items-center justify-center"
                    >
                      {platform}
                    </button>
                  ))}
                </div>

                <div className="border-t pt-4">
                  <h4 className="font-medium mb-2">Active Social Buyers</h4>
                  <div className="space-y-2">
                    <div className="flex items-center justify-between bg-gray-50 p-2 rounded">
                      <div>
                        <p className="font-medium">Sarah K.</p>
                        <p className="text-sm text-gray-600">Looking for grocery partner</p>
                      </div>
                      <button className="text-blue-500 text-sm">Connect</button>
                    </div>
                  </div>
                </div>
              </div>
            </CardContent>
          </Card>
        </div>
      )}
    </div>
  );
};

export default BOGOApp;
