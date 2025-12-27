import React, { useState } from 'react';
import { Monitor, Settings, Image, Cpu, Zap, HelpCircle } from 'lucide-react';

export default function NvidiaControlPanel() {
  const [currentPage, setCurrentPage] = useState('landing'); // 'landing', 'loading', 'app'
  const [activeSection, setActiveSection] = useState('3d-settings');
  const [showPopup, setShowPopup] = useState(false);
  const [showAbout, setShowAbout] = useState(false);
  const [popupMessage, setPopupMessage] = useState({ title: '', description: '' });
  const [carPosition, setCarPosition] = useState(0);
  const [settings, setSettings] = useState({
    powerMode: 'adaptive',
    vsync: 'off',
    maxFrameRate: '1000',
    antiAliasing: '4x',
    textureFiltering: 'high',
    ambientOcclusion: 'quality',
    anisotropicFiltering: '16x',
    lowLatencyMode: 'off',
    brightness: 50,
    contrast: 50,
    gamma: 1.0,
    digitalVibrance: 50,
    resolution: '1920x1080',
    refreshRate: '144',
    colorDepth: '32-bit',
    gsyncEnabled: true
  });

  const updateSetting = (key, value) => {
    setSettings(prev => ({ ...prev, [key]: value }));
  };

  const handlePowerModeChange = (value) => {
    let title = '';
    let description = '';
    
    switch(value) {
      case 'adaptive':
        title = 'Adaptive Power Mode';
        description = 'Automatically balances performance and power consumption based on workload. Recommended for general use.';
        break;
      case 'prefer-max':
        title = 'Prefer Maximum Performance';
        description = 'GPU runs at maximum clock speeds at all times. Provides best performance but increases power consumption and heat generation.';
        break;
      case 'optimal':
        title = 'Optimal Power Mode';
        description = 'Optimizes power efficiency while maintaining good performance. Best for laptop users and energy savings.';
        break;
    }
    
    setPopupMessage({ title, description });
    setShowPopup(true);
    updateSetting('powerMode', value);
  };

  const handleRefreshRateChange = (value) => {
    updateSetting('refreshRate', value);
  };

  const handleStart = () => {
    setCurrentPage('loading');
    setCarPosition(0);
    
    // Animate car from 0 to 100% over 3 seconds
    const duration = 3000;
    const interval = 20;
    const steps = duration / interval;
    const increment = 100 / steps;
    
    let currentStep = 0;
    const timer = setInterval(() => {
      currentStep++;
      setCarPosition(prev => Math.min(prev + increment, 100));
      
      if (currentStep >= steps) {
        clearInterval(timer);
        setTimeout(() => setCurrentPage('app'), 500);
      }
    }, interval);
  };

  // Landing Page
  if (currentPage === 'landing') {
    return (
      <div className="w-full h-screen bg-gradient-to-br from-green-600 via-green-700 to-green-800 flex items-center justify-center">
        <div className="text-center">
          <div className="mb-8">
            <div className="w-32 h-32 bg-white rounded-full flex items-center justify-center mx-auto mb-6 shadow-2xl">
              <span className="text-green-600 font-bold text-6xl">N</span>
            </div>
            <h1 className="text-5xl font-bold text-white mb-4">NVIDIA Control Panel</h1>
            <p className="text-green-100 text-xl mb-8">Graphics Settings Simulator</p>
          </div>
          
          <button 
            onClick={handleStart}
            className="bg-white text-green-700 px-12 py-4 rounded-lg text-xl font-semibold hover:bg-green-50 transform hover:scale-105 transition-all shadow-xl"
          >
            Start
          </button>
          
          <div className="mt-8 text-green-100 text-sm">
            Training Tool by Nambirajan
          </div>
        </div>
      </div>
    );
  }

  // Loading Page with F1 Car Animation
  if (currentPage === 'loading') {
    return (
      <div className="w-full h-screen bg-gray-900 flex items-center justify-center overflow-hidden">
        <div className="w-full max-w-4xl px-8">
          <div className="text-center mb-12">
            <h2 className="text-3xl font-bold text-white mb-2">Starting NVIDIA Control Panel</h2>
            <p className="text-gray-400">Please wait...</p>
          </div>
          
          {/* Race Track */}
          <div className="relative h-32 bg-gray-800 rounded-lg overflow-hidden border-4 border-gray-700">
            {/* Track Lines */}
            <div className="absolute inset-0 flex items-center">
              <div className="w-full h-1 bg-yellow-400 opacity-50"></div>
            </div>
            
            {/* Start Line */}
            <div className="absolute left-0 top-0 bottom-0 w-2 bg-white"></div>
            
            {/* Finish Line */}
            <div className="absolute right-0 top-0 bottom-0 w-2 bg-white flex flex-col">
              <div className="flex-1 bg-black"></div>
              <div className="flex-1 bg-white"></div>
              <div className="flex-1 bg-black"></div>
              <div className="flex-1 bg-white"></div>
            </div>
            
            {/* F1 Car */}
            <div 
              className="absolute top-1/2 transform -translate-y-1/2 transition-all duration-75 ease-linear"
              style={{ left: `${carPosition}%` }}
            >
              <div className="relative">
                {/* Car Body */}
                <svg width="80" height="40" viewBox="0 0 80 40" className="drop-shadow-lg">
                  {/* Main Body */}
                  <path d="M10,20 L20,15 L50,15 L70,20 L70,25 L10,25 Z" fill="#00ff00" stroke="#006600" strokeWidth="1"/>
                  {/* Cockpit */}
                  <rect x="25" y="12" width="15" height="8" fill="#222" rx="2"/>
                  {/* Front Wing */}
                  <rect x="5" y="18" width="8" height="4" fill="#00cc00" rx="1"/>
                  {/* Rear Wing */}
                  <rect x="68" y="10" width="8" height="15" fill="#00cc00" rx="1"/>
                  {/* Wheels */}
                  <circle cx="20" cy="25" r="5" fill="#222"/>
                  <circle cx="60" cy="25" r="5" fill="#222"/>
                  {/* Wheel Details */}
                  <circle cx="20" cy="25" r="3" fill="#444"/>
                  <circle cx="60" cy="25" r="3" fill="#444"/>
                </svg>
                
                {/* Speed Lines */}
                {carPosition > 5 && (
                  <div className="absolute -left-8 top-1/2 transform -translate-y-1/2">
                    <div className="flex gap-1">
                      <div className="w-4 h-1 bg-white opacity-50"></div>
                      <div className="w-3 h-1 bg-white opacity-30"></div>
                      <div className="w-2 h-1 bg-white opacity-20"></div>
                    </div>
                  </div>
                )}
              </div>
            </div>
          </div>
          
          {/* Progress Bar */}
          <div className="mt-8">
            <div className="w-full h-2 bg-gray-700 rounded-full overflow-hidden">
              <div 
                className="h-full bg-green-500 transition-all duration-75 ease-linear"
                style={{ width: `${carPosition}%` }}
              ></div>
            </div>
            <div className="text-center mt-4 text-gray-400">
              {Math.round(carPosition)}%
            </div>
          </div>
        </div>
      </div>
    );
  }

  // Main Application

  const menuItems = [
    { id: '3d-settings', label: '3D Settings', icon: Cpu },
    { id: 'display', label: 'Display', icon: Monitor },
    { id: 'color', label: 'Adjust Desktop Color Settings', icon: Image },
    { id: 'video', label: 'Adjust Video Settings', icon: Settings },
    { id: 'power', label: 'Manage 3D Settings', icon: Zap }
  ];

  const renderContent = () => {
    switch(activeSection) {
      case '3d-settings':
        return (
          <div className="space-y-6">
            <h2 className="text-xl font-semibold text-gray-800">Manage 3D Settings</h2>
            
            <div className="bg-gray-50 p-4 rounded">
              <h3 className="font-semibold mb-4">Global Settings</h3>
              
              <div className="space-y-4">
                <SettingRow label="Power Management Mode">
                  <select 
                    value={settings.powerMode}
                    onChange={(e) => handlePowerModeChange(e.target.value)}
                    className="border rounded px-3 py-1 bg-white"
                  >
                    <option value="adaptive">Adaptive</option>
                    <option value="prefer-max">Prefer Maximum Performance</option>
                    <option value="optimal">Optimal Power</option>
                  </select>
                </SettingRow>

                <SettingRow label="Vertical Sync">
                  <select 
                    value={settings.vsync}
                    onChange={(e) => updateSetting('vsync', e.target.value)}
                    className="border rounded px-3 py-1 bg-white"
                  >
                    <option value="off">Off</option>
                    <option value="on">On</option>
                    <option value="adaptive">Adaptive</option>
                    <option value="fast">Fast</option>
                  </select>
                </SettingRow>

                <div className="py-2 border-b border-gray-200">
                  <SliderSetting 
                    label="Max Frame Rate"
                    value={settings.maxFrameRate === 'unlimited' ? 1000 : parseInt(settings.maxFrameRate)}
                    onChange={(val) => updateSetting('maxFrameRate', val.toString())}
                    min={30}
                    max={1000}
                    step={10}
                  />
                  <div className="text-xs text-gray-500 mt-1 text-right">
                    {settings.maxFrameRate === '1000' ? 'Unlimited' : `${settings.maxFrameRate} FPS`}
                  </div>
                </div>

                <SettingRow label="Antialiasing - Mode">
                  <select 
                    value={settings.antiAliasing}
                    onChange={(e) => updateSetting('antiAliasing', e.target.value)}
                    className="border rounded px-3 py-1 bg-white"
                  >
                    <option value="off">Off</option>
                    <option value="2x">2x</option>
                    <option value="4x">4x</option>
                    <option value="8x">8x</option>
                    <option value="16x">16x</option>
                  </select>
                </SettingRow>

                <SettingRow label="Texture Filtering - Quality">
                  <select 
                    value={settings.textureFiltering}
                    onChange={(e) => updateSetting('textureFiltering', e.target.value)}
                    className="border rounded px-3 py-1 bg-white"
                  >
                    <option value="high">High Quality</option>
                    <option value="quality">Quality</option>
                    <option value="performance">Performance</option>
                  </select>
                </SettingRow>

                <SettingRow label="Ambient Occlusion">
                  <select 
                    value={settings.ambientOcclusion}
                    onChange={(e) => updateSetting('ambientOcclusion', e.target.value)}
                    className="border rounded px-3 py-1 bg-white"
                  >
                    <option value="off">Off</option>
                    <option value="performance">Performance</option>
                    <option value="quality">Quality</option>
                  </select>
                </SettingRow>

                <SettingRow label="Anisotropic Filtering">
                  <select 
                    value={settings.anisotropicFiltering}
                    onChange={(e) => updateSetting('anisotropicFiltering', e.target.value)}
                    className="border rounded px-3 py-1 bg-white"
                  >
                    <option value="off">Off</option>
                    <option value="2x">2x</option>
                    <option value="4x">4x</option>
                    <option value="8x">8x</option>
                    <option value="16x">16x</option>
                  </select>
                </SettingRow>

                <SettingRow label="Low Latency Mode">
                  <select 
                    value={settings.lowLatencyMode}
                    onChange={(e) => updateSetting('lowLatencyMode', e.target.value)}
                    className="border rounded px-3 py-1 bg-white"
                  >
                    <option value="off">Off</option>
                    <option value="on">On</option>
                    <option value="ultra">Ultra</option>
                  </select>
                </SettingRow>
              </div>
            </div>

            <div className="flex gap-3">
              <button className="bg-green-600 text-white px-6 py-2 rounded hover:bg-green-700">
                Apply
              </button>
              <button className="bg-gray-400 text-white px-6 py-2 rounded hover:bg-gray-500">
                Reset
              </button>
            </div>
          </div>
        );

      case 'display':
        const getScreenDimensions = (resolution) => {
          const resMap = {
            '1920x1080': { width: 320, height: 180 },
            '2560x1440': { width: 340, height: 191 },
            '3840x2160': { width: 360, height: 202 },
            '1680x1050': { width: 336, height: 210 },
            '1600x900': { width: 320, height: 180 }
          };
          return resMap[resolution] || { width: 320, height: 180 };
        };
        
        const screenSize = getScreenDimensions(settings.resolution);
        
        return (
          <div className="space-y-6">
            <h2 className="text-xl font-semibold text-gray-800">Change Resolution</h2>
            
            <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
              {/* Monitor Preview */}
              <div className="bg-gray-50 p-6 rounded">
                <h3 className="font-semibold mb-4 text-center">Display Preview</h3>
                <div className="flex justify-center items-end">
                  <div className="relative">
                    {/* Monitor Stand */}
                    <div className="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-16 h-3 bg-gray-700 rounded-t"></div>
                    <div className="absolute -bottom-2 left-1/2 transform -translate-x-1/2 w-24 h-2 bg-gray-600 rounded"></div>
                    
                    {/* Monitor Frame */}
                    <div className="bg-gray-800 p-3 rounded-lg shadow-xl">
                      {/* Monitor Screen - Dynamic Size */}
                      <div 
                        className="rounded overflow-hidden relative transition-all duration-500"
                        style={{
                          width: `${screenSize.width}px`,
                          height: `${screenSize.height}px`,
                          background: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)'
                        }}
                      >
                        {/* Sample Desktop Content */}
                        <div className="absolute inset-0 p-4">
                          {/* Top Bar */}
                          <div className="bg-gray-800 bg-opacity-60 rounded px-3 py-1 mb-2 flex items-center justify-between">
                            <div className="flex gap-2">
                              <div className="w-2 h-2 bg-red-500 rounded-full"></div>
                              <div className="w-2 h-2 bg-yellow-500 rounded-full"></div>
                              <div className="w-2 h-2 bg-green-500 rounded-full"></div>
                            </div>
                            <div className="text-white text-xs opacity-75">Desktop</div>
                          </div>
                          
                          {/* Content Area with animated elements based on refresh rate */}
                          <div className="grid grid-cols-3 gap-2 mb-2">
                            <div 
                              className="bg-white bg-opacity-20 rounded p-2 backdrop-blur-sm transition-all"
                              style={{
                                animation: `pulse ${1000 / parseInt(settings.refreshRate)}s ease-in-out infinite`
                              }}
                            >
                              <div className="w-full h-8 bg-white bg-opacity-40 rounded"></div>
                            </div>
                            <div className="bg-white bg-opacity-20 rounded p-2 backdrop-blur-sm">
                              <div className="w-full h-8 bg-white bg-opacity-40 rounded"></div>
                            </div>
                            <div className="bg-white bg-opacity-20 rounded p-2 backdrop-blur-sm">
                              <div className="w-full h-8 bg-white bg-opacity-40 rounded"></div>
                            </div>
                          </div>
                          
                          {/* Moving indicator showing refresh rate smoothness */}
                          <div className="relative h-1 bg-gray-700 bg-opacity-40 rounded-full overflow-hidden">
                            <div 
                              className="absolute h-full bg-green-400 rounded-full"
                              style={{
                                width: '20%',
                                animation: `slide ${2000 / parseInt(settings.refreshRate)}s linear infinite`
                              }}
                            ></div>
                          </div>
                          
                          {/* Bottom section */}
                          <div className="absolute bottom-4 left-4 right-4">
                            <div className="bg-gray-800 bg-opacity-40 rounded p-2 backdrop-blur-sm">
                              <div className="h-1 bg-white bg-opacity-60 rounded mb-1"></div>
                              <div className="h-1 bg-white bg-opacity-40 rounded w-3/4"></div>
                            </div>
                          </div>
                        </div>
                        
                        {/* G-SYNC indicator */}
                        {settings.gsyncEnabled && (
                          <div className="absolute top-2 right-2">
                            <div className="w-2 h-2 bg-green-400 rounded-full animate-pulse"></div>
                          </div>
                        )}
                        
                        {/* Screen glare effect */}
                        <div className="absolute inset-0 bg-gradient-to-br from-white to-transparent opacity-10 pointer-events-none"></div>
                      </div>
                      
                      <style jsx>{`
                        @keyframes slide {
                          0% { left: 0%; }
                          100% { left: 80%; }
                        }
                      `}</style>
                    </div>
                  </div>
                </div>
                
                <div className="mt-4 text-sm text-gray-600 text-center">
                  <div className="font-semibold">Screen Dimensions</div>
                  <div className="text-xs mt-1">
                    Width: {settings.resolution.split('x')[0]}px | Height: {settings.resolution.split('x')[1]}px
                  </div>
                  <div className="text-xs mt-1">
                    {parseInt(settings.refreshRate) >= 144 ? '🎮 Gaming Optimized' : 
                     parseInt(settings.refreshRate) >= 120 ? '⚡ High Performance' : 
                     '📺 Standard'}
                  </div>
                </div>
              </div>

              {/* Settings Panel */}
              <div className="bg-gray-50 p-4 rounded">
                <h3 className="font-semibold mb-4">Display Configuration</h3>
                <div className="space-y-4">
                  <SettingRow label="Resolution">
                    <select 
                      value={settings.resolution}
                      onChange={(e) => updateSetting('resolution', e.target.value)}
                      className="border rounded px-3 py-1 bg-white"
                    >
                      <option value="1920x1080">1920 x 1080 (recommended)</option>
                      <option value="2560x1440">2560 x 1440</option>
                      <option value="3840x2160">3840 x 2160 (4K)</option>
                      <option value="1680x1050">1680 x 1050</option>
                      <option value="1600x900">1600 x 900</option>
                    </select>
                  </SettingRow>

                  <SettingRow label="Refresh Rate">
                    <select 
                      value={settings.refreshRate}
                      onChange={(e) => handleRefreshRateChange(e.target.value)}
                      className="border rounded px-3 py-1 bg-white"
                    >
                      <option value="60">60 Hz</option>
                      <option value="120">120 Hz</option>
                      <option value="120">120 Hz</option>
                      <option value="144">144 Hz</option>
                      <option value="165">165 Hz</option>
                      <option value="240">240 Hz</option>
                    </select>
                  </SettingRow>

                  <SettingRow label="Color Depth">
                    <select 
                      value={settings.colorDepth}
                      onChange={(e) => updateSetting('colorDepth', e.target.value)}
                      className="border rounded px-3 py-1 bg-white"
                    >
                      <option value="32-bit">32-bit (highest quality)</option>
                      <option value="16-bit">16-bit</option>
                    </select>
                  </SettingRow>

                  <SettingRow label="G-SYNC">
                    <label className="flex items-center gap-2">
                      <input 
                        type="checkbox"
                        checked={settings.gsyncEnabled}
                        onChange={(e) => updateSetting('gsyncEnabled', e.target.checked)}
                        className="w-4 h-4"
                      />
                      <span>Enable G-SYNC</span>
                    </label>
                  </SettingRow>
                </div>
              </div>
            </div>

            <div className="flex gap-3">
              <button className="bg-green-600 text-white px-6 py-2 rounded hover:bg-green-700">
                Apply
              </button>
            </div>
          </div>
        );

      case 'color':
        return (
          <div className="space-y-6">
            <h2 className="text-xl font-semibold text-gray-800">Adjust Desktop Color Settings</h2>
            
            <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
              {/* Monitor Preview */}
              <div className="bg-gray-50 p-6 rounded">
                <h3 className="font-semibold mb-4 text-center">Preview</h3>
                <div className="flex justify-center">
                  <div className="relative">
                    {/* Monitor Stand */}
                    <div className="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-16 h-3 bg-gray-700 rounded-t"></div>
                    <div className="absolute -bottom-2 left-1/2 transform -translate-x-1/2 w-24 h-2 bg-gray-600 rounded"></div>
                    
                    {/* Monitor Frame */}
                    <div className="bg-gray-800 p-3 rounded-lg shadow-xl">
                      {/* Monitor Screen */}
                      <div 
                        className="w-80 h-48 rounded overflow-hidden relative"
                        style={{
                          filter: `brightness(${settings.brightness}%) contrast(${settings.contrast}%) saturate(${50 + settings.digitalVibrance}%)`,
                          opacity: Math.min(1, 0.5 + (settings.gamma / 2))
                        }}
                      >
                        {/* Background gradient */}
                        <div className="absolute inset-0 bg-gradient-to-br from-blue-400 via-purple-500 to-pink-500"></div>
                        
                        {/* Sample content */}
                        <div className="absolute inset-0 p-4">
                          <div className="bg-white bg-opacity-90 rounded p-3 mb-2">
                            <div className="h-2 bg-gray-800 rounded mb-2"></div>
                            <div className="h-2 bg-gray-600 rounded w-3/4"></div>
                          </div>
                          
                          <div className="grid grid-cols-3 gap-2">
                            <div className="bg-red-500 h-12 rounded"></div>
                            <div className="bg-green-500 h-12 rounded"></div>
                            <div className="bg-blue-500 h-12 rounded"></div>
                          </div>
                          
                          <div className="mt-2 bg-white bg-opacity-90 rounded p-2">
                            <div className="h-1 bg-gray-700 rounded mb-1"></div>
                            <div className="h-1 bg-gray-500 rounded w-2/3"></div>
                          </div>
                        </div>
                        
                        {/* Screen glare effect */}
                        <div className="absolute inset-0 bg-gradient-to-br from-white to-transparent opacity-10"></div>
                      </div>
                    </div>
                  </div>
                </div>
                
                <div className="mt-4 text-sm text-gray-600 text-center">
                  <div>Live Preview</div>
                  <div className="text-xs mt-1">Adjust settings to see changes</div>
                </div>
              </div>

              {/* Settings Panel */}
              <div className="bg-gray-50 p-4 rounded">
                <h3 className="font-semibold mb-4">Color Adjustments</h3>
                <div className="space-y-6">
                  <SliderSetting 
                    label="Brightness"
                    value={settings.brightness}
                    onChange={(val) => updateSetting('brightness', val)}
                    min={0}
                    max={100}
                  />

                  <SliderSetting 
                    label="Contrast"
                    value={settings.contrast}
                    onChange={(val) => updateSetting('contrast', val)}
                    min={0}
                    max={100}
                  />

                  <SliderSetting 
                    label="Gamma"
                    value={settings.gamma}
                    onChange={(val) => updateSetting('gamma', val)}
                    min={0.5}
                    max={2.0}
                    step={0.1}
                  />

                  <SliderSetting 
                    label="Digital Vibrance"
                    value={settings.digitalVibrance}
                    onChange={(val) => updateSetting('digitalVibrance', val)}
                    min={0}
                    max={100}
                  />
                </div>
              </div>
            </div>

            <div className="flex gap-3">
              <button className="bg-green-600 text-white px-6 py-2 rounded hover:bg-green-700">
                Apply
              </button>
              <button 
                className="bg-gray-400 text-white px-6 py-2 rounded hover:bg-gray-500"
                onClick={() => {
                  updateSetting('brightness', 50);
                  updateSetting('contrast', 50);
                  updateSetting('gamma', 1.0);
                  updateSetting('digitalVibrance', 50);
                }}
              >
                Reset to Default
              </button>
            </div>
          </div>
        );

      default:
        return (
          <div className="space-y-4">
            <h2 className="text-xl font-semibold text-gray-800">
              {menuItems.find(item => item.id === activeSection)?.label}
            </h2>
            <p className="text-gray-600">Settings panel for {activeSection}</p>
          </div>
        );
    }
  };

  return (
    <div className="w-full h-screen bg-gray-100 flex flex-col">
      {/* About Modal */}
      {showAbout && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
          <div className="bg-white rounded-lg shadow-xl p-8 max-w-md w-full mx-4">
            <div className="text-center">
              <div className="w-16 h-16 bg-green-600 rounded-full flex items-center justify-center mx-auto mb-4">
                <span className="text-white font-bold text-2xl">N</span>
              </div>
              <h3 className="text-xl font-semibold text-gray-800 mb-2">NVIDIA Control Panel</h3>
              <p className="text-gray-600 mb-6">Training Simulator</p>
              
              <div className="border-t border-gray-200 pt-4 mb-6">
                <div className="text-sm text-gray-700 mb-2">
                  <span className="font-semibold">Creator:</span> Nambirajan
                </div>
                <div className="text-sm text-gray-700">
                  <span className="font-semibold">Purpose:</span> Training for Lenovo Agent
                </div>
              </div>
              
              <button 
                onClick={() => setShowAbout(false)}
                className="bg-green-600 text-white px-6 py-2 rounded hover:bg-green-700 w-full"
              >
                Close
              </button>
            </div>
          </div>
        </div>
      )}

      {/* Popup Modal */}
      {showPopup && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
          <div className="bg-white rounded-lg shadow-xl p-6 max-w-md w-full mx-4">
            <h3 className="text-lg font-semibold text-gray-800 mb-3">{popupMessage.title}</h3>
            <p className="text-gray-600 mb-6">{popupMessage.description}</p>
            <div className="flex justify-end">
              <button 
                onClick={() => setShowPopup(false)}
                className="bg-green-600 text-white px-6 py-2 rounded hover:bg-green-700"
              >
                OK
              </button>
            </div>
          </div>
        </div>
      )}

      {/* Header */}
      <div className="bg-gradient-to-r from-green-600 to-green-700 text-white px-6 py-4 flex items-center justify-between">
        <div className="flex items-center gap-3">
          <div className="w-8 h-8 bg-white rounded flex items-center justify-center">
            <span className="text-green-600 font-bold text-lg">N</span>
          </div>
          <span className="text-xl font-semibold">NVIDIA Control Panel</span>
        </div>
        <button 
          onClick={() => setShowAbout(true)}
          className="hover:bg-green-800 p-2 rounded transition-colors"
        >
          <HelpCircle className="w-5 h-5" />
        </button>
      </div>

      <div className="flex flex-1 overflow-hidden">
        {/* Sidebar */}
        <div className="w-72 bg-white border-r border-gray-200 overflow-y-auto">
          <div className="p-4">
            <div className="text-xs font-semibold text-gray-500 mb-2 uppercase">Settings</div>
            {menuItems.map(item => (
              <button
                key={item.id}
                onClick={() => setActiveSection(item.id)}
                className={`w-full flex items-center gap-3 px-3 py-2 rounded text-left transition-colors ${
                  activeSection === item.id
                    ? 'bg-green-100 text-green-700'
                    : 'hover:bg-gray-100 text-gray-700'
                }`}
              >
                <item.icon className="w-4 h-4" />
                <span className="text-sm">{item.label}</span>
              </button>
            ))}
          </div>

          <div className="p-4 border-t">
            <div className="text-xs text-gray-500">
              <div className="font-semibold mb-2">System Information</div>
              <div>GPU: NVIDIA GeForce RTX 4080</div>
              <div>Driver: 546.33</div>
              <div>CUDA: 12.3</div>
            </div>
          </div>
        </div>

        {/* Main Content */}
        <div className="flex-1 overflow-y-auto p-6">
          {renderContent()}
        </div>
      </div>

      {/* Status Bar */}
      <div className="bg-gray-200 px-6 py-2 text-xs text-gray-600 border-t">
        NVIDIA Control Panel v8.1.962.0 - © 2024 NVIDIA Corporation
      </div>
    </div>
  );
}

function SettingRow({ label, children }) {
  return (
    <div className="flex items-center justify-between py-2 border-b border-gray-200">
      <span className="text-sm text-gray-700 font-medium">{label}</span>
      <div>{children}</div>
    </div>
  );
}

function SliderSetting({ label, value, onChange, min = 0, max = 100, step = 1 }) {
  return (
    <div className="space-y-2">
      <div className="flex justify-between items-center">
        <span className="text-sm text-gray-700 font-medium">{label}</span>
        <span className="text-sm text-gray-600">{value}</span>
      </div>
      <input
        type="range"
        min={min}
        max={max}
        step={step}
        value={value}
        onChange={(e) => onChange(parseFloat(e.target.value))}
        className="w-full h-2 bg-gray-300 rounded-lg appearance-none cursor-pointer accent-green-600"
      />
    </div>
  );
}
