// App.jsx
import React, { useState, useRef, useEffect } from 'react';
import { Camera, Upload, Heart, Shield, Clock, Ruler, Activity, Calendar, Pill, Stethoscope, User, MessageCircle, Send, X, Check } from 'lucide-react';

const App = () => {
  // State management
  const [image, setImage] = useState(null);
  const [analysisResults, setAnalysisResults] = useState(null);
  const [healingHistory, setHealingHistory] = useState([]);
  const [isAnalyzing, setIsAnalyzing] = useState(false);
  const [activeTab, setActiveTab] = useState('camera');
  const [chatMessages, setChatMessages] = useState([]);
  const [newMessage, setNewMessage] = useState('');
  const [isChatOpen, setIsChatOpen] = useState(false);
  const [currentDoctor, setCurrentDoctor] = useState(null);
  const [showCamera, setShowCamera] = useState(false);
  const [cameraError, setCameraError] = useState(null);
  
  const videoRef = useRef(null);
  const canvasRef = useRef(null);
  const streamRef = useRef(null);
  const fileInputRef = useRef(null);
  const chatContainerRef = useRef(null);

  // Virtual doctor personalities
  const doctorPersonalities = [
    {
      id: 1,
      name: "Dr. Elena Rodriguez",
      specialty: "Wound Care Specialist",
      personality: "methodical",
      avatar: "ER"
    },
    {
      id: 2,
      name: "Dr. James Peterson",
      specialty: "Infectious Disease Expert",
      personality: "cautious",
      avatar: "JP"
    },
    {
      id: 3,
      name: "Dr. Sarah Kim",
      specialty: "Regenerative Medicine",
      personality: "optimistic",
      avatar: "SK"
    },
    {
      id: 4,
      name: "Dr. Michael Thompson",
      specialty: "Geriatric Medicine",
      personality: "gentle",
      avatar: "MT"
    }
  ];

  // Initialize camera
  useEffect(() => {
    if (showCamera && activeTab === 'camera') {
      startCamera();
    } else {
      stopCamera();
    }

    return () => {
      stopCamera();
    };
  }, [showCamera, activeTab]);

  // Scroll chat to bottom
  useEffect(() => {
    if (chatContainerRef.current) {
      chatContainerRef.current.scrollTop = chatContainerRef.current.scrollHeight;
    }
  }, [chatMessages, isChatOpen]);

  const startCamera = async () => {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ 
        video: { facingMode: "environment" } 
      });
      streamRef.current = stream;
      if (videoRef.current) {
        videoRef.current.srcObject = stream;
      }
      setCameraError(null);
    } catch (err) {
      console.error("Error accessing camera:", err);
      setCameraError("Could not access camera. Please ensure you've granted permission.");
    }
  };

  const stopCamera = () => {
    if (streamRef.current) {
      streamRef.current.getTracks().forEach(track => track.stop());
      streamRef.current = null;
    }
  };

  const captureImage = () => {
    if (videoRef.current && canvasRef.current) {
      const video = videoRef.current;
      const canvas = canvasRef.current;
      const context = canvas.getContext('2d');
      
      if (context) {
        canvas.width = video.videoWidth;
        canvas.height = video.videoHeight;
        context.drawImage(video, 0, 0, canvas.width, canvas.height);
        const imageData = canvas.toDataURL('image/png');
        setImage(imageData);
        setAnalysisResults(null);
        setShowCamera(false);
      }
    }
  };

  const handleFileUpload = (e) => {
    const file = e.target.files?.[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (event) => {
        if (event.target?.result) {
          setImage(event.target.result);
          setAnalysisResults(null);
        }
      };
      reader.readAsDataURL(file);
    }
  };

  const simulateAIAnalysis = () => {
    setIsAnalyzing(true);
    
    // Simulate API call delay
    setTimeout(() => {
      const previousResult = healingHistory[healingHistory.length - 1];
      
      // Generate realistic analysis results
      const sizeReduction = previousResult 
        ? Math.min(100, Math.max(0, previousResult.sizeReduction + (Math.random() * 10 - 3)))
        : Math.random() * 30;
      
      const redness = Math.max(0, 100 - (healingHistory.length * 8 + Math.random() * 20));
      const pus = Math.max(0, 30 - (healingHistory.length * 3 + Math.random() * 10));
      const infection = Math.max(0, 20 - (healingHistory.length * 2 + Math.random() * 5));
      
      // Calculate healing score (higher is better)
      const healingScore = Math.min(100, Math.max(0, 
        100 - (redness * 0.3 + pus * 0.4 + infection * 0.3)
      ));
      
      // Simulate wound measurements
      const width = previousResult 
        ? Math.max(5, previousResult.width - (Math.random() * 3))
        : 20 + Math.random() * 15;
      
      const depth = previousResult 
        ? Math.max(1, previousResult.depth - (Math.random() * 1.5))
        : 5 + Math.random() * 8;
      
      // Estimate healing days
      let estimatedHealingDays = 0;
      if (healingHistory.length > 1) {
        const firstScore = healingHistory[0].healingScore;
        const currentScore = healingScore;
        const daysTracked = healingHistory.length;
        const avgDailyImprovement = (currentScore - firstScore) / daysTracked;
        
        if (avgDailyImprovement > 0) {
          const remainingImprovement = 100 - currentScore;
          estimatedHealingDays = Math.max(1, Math.round(remainingImprovement / avgDailyImprovement));
        } else {
          estimatedHealingDays = 14 + Math.floor(Math.random() * 10);
        }
      } else {
        estimatedHealingDays = 10 + Math.floor(Math.random() * 15);
      }
      
      // Select a doctor personality
      const doctorIndex = Math.floor(Math.random() * doctorPersonalities.length);
      const selectedDoctor = doctorPersonalities[doctorIndex];
      setCurrentDoctor(selectedDoctor);
      
      const newResult = {
        sizeReduction,
        redness,
        pus,
        infection,
        healingScore,
        width,
        depth,
        estimatedHealingDays,
        date: new Date().toISOString().split('T')[0],
        doctorName: selectedDoctor.name,
        doctorSpecialty: selectedDoctor.specialty,
        doctorAvatar: selectedDoctor.avatar
      };
      
      setAnalysisResults(newResult);
      setHealingHistory(prev => [...prev, newResult]);
      setIsAnalyzing(false);
      
      // Add doctor's initial message to chat
      const initialMessage = {
        id: Date.now(),
        text: Hello! I'm ${selectedDoctor.name}, ${selectedDoctor.specialty}. I've analyzed your wound and have some personalized advice for you. Your healing score is ${healingScore.toFixed(1)}/100, which indicates ${healingScore > 80 ? "excellent" : healingScore > 60 ? "good" : "moderate"} progress.,
        sender: "doctor",
        timestamp: new Date()
      };
      
      setChatMessages([initialMessage]);
    }, 1500);
  };

  const getHealingTrend = () => {
    if (healingHistory.length < 2) return "Not enough data";
    
    const firstScore = healingHistory[0].healingScore;
    const lastScore = healingHistory[healingHistory.length - 1].healingScore;
    const diff = lastScore - firstScore;
    
    if (diff > 10) return "Improving significantly";
    if (diff > 0) return "Showing improvement";
    if (diff < -10) return "Deteriorating";
    if (diff < 0) return "Slight decline";
    return "Stable";
  };

  const getHealingColor = (score) => {
    if (score > 80) return "bg-green-500";
    if (score > 60) return "bg-yellow-500";
    return "bg-red-500";
  };

  const handleSendMessage = () => {
    if (!newMessage.trim() || !currentDoctor) return;
    
    // Add user message
    const userMessage = {
      id: Date.now(),
      text: newMessage,
      sender: "user",
      timestamp: new Date()
    };
    
    setChatMessages(prev => [...prev, userMessage]);
    setNewMessage("");
    
    // Simulate doctor response after a delay
    setTimeout(() => {
      const responses = [
        "I recommend continuing with your current treatment plan.",
        "Based on your symptoms, we should consider adjusting your medication.",
        "Your healing progress is on track. Keep up the good work!",
        "I'm concerned about the infection levels. Let's schedule a follow-up.",
        "Make sure to keep the wound clean and dry at all times.",
        "Have you been following the prescribed diet? Proper nutrition is crucial.",
        "Any changes in pain levels or discharge?",
        "I suggest increasing the frequency of wound cleaning to twice daily.",
        "The redness is reducing, which is a positive sign.",
        "Let's monitor the pus levels closely over the next few days."
      ];
      
      const doctorResponse = {
        id: Date.now() + 1,
        text: responses[Math.floor(Math.random() * responses.length)],
        sender: "doctor",
        timestamp: new Date()
      };
      
      setChatMessages(prev => [...prev, doctorResponse]);
    }, 1000);
  };

  const handleKeyPress = (e) => {
    if (e.key === "Enter" && !e.shiftKey) {
      e.preventDefault();
      handleSendMessage();
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 p-4 md:p-6">
      <div className="max-w-6xl mx-auto">
        {/* Header */}
        <header className="mb-8 text-center">
          <h1 className="text-3xl md:text-4xl font-bold text-gray-800 mb-2 flex items-center justify-center">
            <Shield className="h-8 w-8 text-blue-600 mr-3" />
            AI Wound Healing Monitor
          </h1>
          <p className="text-gray-600">Track your wound recovery progress with AI-powered analysis</p>
        </header>

        <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          {/* Left Column - Image Capture */}
          <div className="lg:col-span-1 space-y-6">
            <div className="bg-white rounded-xl shadow-lg p-6">
              <div className="flex items-center gap-2 mb-4">
                <Camera className="h-5 w-5 text-blue-600" />
                <h2 className="text-xl font-bold text-gray-800">Capture Wound Image</h2>
              </div>
              
              <div className="flex border-b mb-4">
                <button
                  className={`flex-1 py-2 px-4 text-center ${
                    activeTab === "camera"
                      ? "border-b-2 border-blue-500 text-blue-600 font-medium"
                      : "text-gray-500"
                  }`}
                  onClick={() => {
                    setActiveTab("camera");
                    setShowCamera(true);
                  }}
                >
                  Camera
                </button>
                <button
                  className={`flex-1 py-2 px-4 text-center ${
                    activeTab === "upload"
                      ? "border-b-2 border-blue-500 text-blue-600 font-medium"
                      : "text-gray-500"
                  }`}
                  onClick={() => {
                    setActiveTab("upload");
                    setShowCamera(false);
                  }}
                >
                  Upload
                </button>
              </div>

              {activeTab === "camera" && (
                <div className="mb-4">
                  {showCamera ? (
                    <div className="relative">
                      <video 
                        ref={videoRef}
                        autoPlay
                        playsInline
                        className="w-full h-48 object-cover rounded-lg bg-black"
                      />
                      <canvas ref={canvasRef} className="hidden" />
                      
                      {cameraError ? (
                        <div className="absolute inset-0 flex items-center justify-center bg-black bg-opacity-70 text-white p-4 text-center">
                          <p>{cameraError}</p>
                        </div>
                      ) : (
                        <div className="absolute bottom-4 left-0 right-0 flex justify-center">
                          <button 
                            onClick={captureImage}
                            className="bg-white text-black rounded-full p-3 hover:bg-gray-100 shadow-lg"
                          >
                            <Camera className="h-6 w-6" />
                          </button>
                        </div>
                      )}
                      
                      <button 
                        className="absolute top-2 right-2 bg-black bg-opacity-30 text-white rounded-full p-2 hover:bg-opacity-50"
                        onClick={() => setShowCamera(false)}
                      >
                        <X className="h-5 w-5" />
                      </button>
                    </div>
                  ) : (
                    <div className="flex flex-col items-center gap-4">
                      <div className="w-full h-48 bg-gray-100 rounded-lg border-2 border-dashed border-gray-300 flex items-center justify-center">
                        <div className="text-center p-4">
                          <Camera className="h-12 w-12 text-gray-400 mx-auto mb-2" />
                          <p className="text-gray-500">Camera not active</p>
                        </div>
                      </div>
                      <button 
                        onClick={() => setShowCamera(true)}
                        className="w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition"
                      >
                        <Camera className="h-4 w-4 inline mr-2" />
                        Start Camera
                      </button>
                    </div>
                  )}
                </div>
              )}

              {activeTab === "upload" && (
                <div className="mb-4">
                  {image ? (
                    <div className="relative">
                      <img 
                        src={image} 
                        alt="Wound preview" 
                        className="w-full h-48 object-contain rounded-lg border"
                      />
                      <button 
                        className="absolute top-2 right-2 bg-black bg-opacity-30 text-white rounded-full p-2 hover:bg-opacity-50"
                        onClick={() => setImage(null)}
                      >
                        <X className="h-5 w-5" />
                      </button>
                    </div>
                  ) : (
                    <div className="w-full h-48 bg-gray-100 rounded-lg border-2 border-dashed border-gray-300 flex items-center justify-center">
                      <div className="text-center p-4">
                        <Upload className="h-12 w-12 text-gray-400 mx-auto mb-2" />
                        <p className="text-gray-500">No image uploaded</p>
                      </div>
                    </div>
                  )}
                  
                  <input
                    type="file"
                    ref={fileInputRef}
                    onChange={handleFileUpload}
                    accept="image/*"
                    className="hidden"
                  />
                  
                  <button 
                    onClick={() => fileInputRef.current?.click()}
                    className="w-full mt-4 bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition"
                  >
                    <Upload className="h-4 w-4 inline mr-2" />
                    Select Image
                  </button>
                </div>
              )}

              {image && (
                <button 
                  onClick={simulateAIAnalysis}
                  disabled={isAnalyzing}
                  className="w-full bg-green-600 text-white py-3 rounded-lg hover:bg-green-700 transition flex items-center justify-center disabled:opacity-70"
                >
                  {isAnalyzing ? (
                    <>
                      <div className="mr-2 h-4 w-4 animate-spin rounded-full border-2 border-current border-t-transparent"></div>
                      Analyzing...
                    </>
                  ) : (
                    <>
                      <Shield className="h-4 w-4 mr-2" />
                      Analyze Wound
                    </>
                  )}
                </button>
              )}
              
              <p className="text-sm text-gray-500 mt-3">
                For best results, ensure good lighting and consistent positioning
              </p>
            </div>
            
            {/* Healing Progress Card */}
            <div className="bg-white rounded-xl shadow-lg p-6">
              <div className="flex items-center gap-2 mb-4">
                <Heart className="h-5 w-5 text-red-600" />
                <h2 className="text-xl font-bold text-gray-800">Healing Progress</h2>
              </div>
              
              {healingHistory.length > 0 ? (
                <div>
                  <div className="flex justify-between mb-2">
                    <span className="font-medium">Current Status</span>
                    <span className="font-bold text-lg">
                      {healingHistory[healingHistory.length - 1].healingScore.toFixed(1)}/100
                    </span>
                  </div>
                  
                  <div className="w-full bg-gray-200 rounded-full h-2.5 mb-4">
                    <div 
                      className={h-2.5 rounded-full ${getHealingColor(healingHistory[healingHistory.length - 1].healingScore)}} 
                      style={{ width: ${healingHistory[healingHistory.length - 1].healingScore}% }}
                    ></div>
                  </div>
                  
                  <div className="flex justify-between text-sm">
                    <span>Days Tracked: {healingHistory.length}</span>
                    <span className="font-medium">
                      {getHealingTrend()}
                    </span>
                  </div>
                  
                  <div className="mt-4">
                    <h3 className="font-medium mb-2">Measurements</h3>
                    <div className="grid grid-cols-2 gap-3">
                      <div className="bg-blue-50 p-3 rounded-lg">
                        <div className="flex items-center">
                          <Ruler className="h-4 w-4 text-blue-600 mr-2" />
                          <span className="text-sm">Width</span>
                        </div>
                        <p className="font-bold text-blue-700">
                          {healingHistory[healingHistory.length - 1].width.toFixed(1)} mm
                        </p>
                      </div>
                      <div className="bg-indigo-50 p-3 rounded-lg">
                        <div className="flex items-center">
                          <Activity className="h-4 w-4 text-indigo-600 mr-2" />
                          <span className="text-sm">Depth</span>
                        </div>
                        <p className="font-bold text-indigo-700">
                          {healingHistory[healingHistory.length - 1].depth.toFixed(1)} mm
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
              ) : (
                <div className="text-center py-6">
                  <Heart className="h-12 w-12 text-gray-300 mx-auto mb-3" />
                  <p className="text-gray-500">
                    No healing data yet. Capture and analyze your first wound image to start tracking progress.
                  </p>
                </div>
              )}
            </div>
          </div>
          
          {/* Middle Column - Analysis Results */}
          <div className="lg:col-span-1 space-y-6">
            <div className="bg-white rounded-xl shadow-lg p-6">
              <div className="flex items-center gap-2 mb-4">
                <Shield className="h-5 w-5 text-green-600" />
                <h2 className="text-xl font-bold text-gray-800">Analysis Results</h2>
              </div>
              
              {analysisResults ? (
                <div className="space-y-5">
                  <div className="grid grid-cols-2 gap-4">
                    <div className="bg-blue-50 p-4 rounded-lg">
                      <p className="text-sm text-gray-600">Size Reduction</p>
                      <p className="text-2xl font-bold text-blue-700">
                        {analysisResults.sizeReduction.toFixed(1)}%
                      </p>
                    </div>
                    <div className="bg-red-50 p-4 rounded-lg">
                      <p className="text-sm text-gray-600">Redness Level</p>
                      <p className="text-2xl font-bold text-red-700">
                        {analysisResults.redness.toFixed(1)}%
                      </p>
                    </div>
                    <div className="bg-yellow-50 p-4 rounded-lg">
                      <p className="text-sm text-gray-600">Pus Presence</p>
                      <p className="text-2xl font-bold text-yellow-700">
                        {analysisResults.pus.toFixed(1)}%
                      </p>
                    </div>
                    <div className="bg-purple-50 p-4 rounded-lg">
                      <p className="text-sm text-gray-600">Infection Risk</p>
                      <p className="text-2xl font-bold text-purple-700">
                        {analysisResults.infection.toFixed(1)}%
                      </p>
                    </div>
                  </div>
                  
                  <div className="bg-green-50 p-4 rounded-lg">
                    <div className="flex items-center gap-2 mb-1">
                      <Calendar className="h-5 w-5 text-green-600" />
                      <p className="text-sm text-gray-600 font-medium">Estimated Healing Time</p>
                    </div>
                    <p className="text-2xl font-bold text-green-700">
                      {analysisResults.estimatedHealingDays} days
                    </p>
                  </div>
                  
                  <div className="pt-2">
                    <div className="flex justify-between mb-2">
                      <span className="font-medium">Healing Score</span>
                      <span className="font-bold text-lg">
                        {analysisResults.healingScore.toFixed(1)}/100
                      </span>
                    </div>
                    <div className="w-full bg-gray-200 rounded-full h-2.5">
                      <div 
                        className={h-2.5 rounded-full ${getHealingColor(analysisResults.healingScore)}} 
                        style={{ width: ${analysisResults.healingScore}% }}
                      ></div>
                    </div>
                    <p className="text-sm text-gray-600 mt-2">
                      {analysisResults.healingScore > 80 
                        ? "Excellent healing progress" 
                        : analysisResults.healingScore > 60 
                          ? "Good healing progress" 
                          : "Requires attention"}
                    </p>
                  </div>
                  
                  <div className="bg-indigo-50 p-4 rounded-lg">
                    <div className="flex items-start gap-3">
                      <div className="bg-indigo-100 w-10 h-10 rounded-full flex items-center justify-center">
                        <span className="font-bold text-indigo-700">{analysisResults.doctorAvatar}</span>
                      </div>
                      <div>
                        <div className="flex items-center gap-2 mb-1">
                          <h3 className="font-medium text-indigo-700">
                            {analysisResults.doctorName}
                          </h3>
                          <span className="text-xs bg-indigo-100 text-indigo-800 px-2 py-1 rounded">
                            {analysisResults.doctorSpecialty}
                          </span>
                        </div>
                        <p className="text-indigo-800 text-sm">
                          Your healing is progressing well. Continue with the current treatment plan and maintain proper wound hygiene.
                        </p>
                      </div>
                    </div>
                  </div>
                  
                  {currentDoctor && (
                    <button 
                      onClick={() => setIsChatOpen(true)}
                      className="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700 transition flex items-center justify-center"
                    >
                      <MessageCircle className="h-4 w-4 mr-2" />
                      Chat with {currentDoctor.name}
                    </button>
                  )}
                </div>
              ) : (
                <div className="text-center py-10">
                  <Shield className="h-16 w-16 text-gray-300 mx-auto mb-4" />
                  <p className="text-gray-500">
                    {image 
                      ? "Click 'Analyze Wound' to get AI-powered assessment" 
                      : "Capture or upload an image to begin analysis"}
                  </p>
                </div>
              )}
            </div>
            
            {/* Healing History Chart */}
            <div className="bg-white rounded-xl shadow-lg p-6">
              <div className="flex items-center gap-2 mb-4">
                <Activity className="h-5 w-5 text-purple-600" />
                <h2 className="text-xl font-bold text-gray-800">Healing Timeline</h2>
              </div>
              
              {healingHistory.length > 0 ? (
                <div>
                  <div className="h-40 flex items-end gap-2 border-b border-l border-gray-200 p-4">
                    {healingHistory.map((entry, index) => (
                      <div key={index} className="flex flex-col items-center flex-1">
                        <div 
                          className="w-full rounded-t bg-gradient-to-t from-blue-500 to-blue-300"
                          style={{ height: ${entry.healingScore}% }}
                        ></div>
                        <span className="text-xs text-gray-500 mt-1">
                          {entry.date}
                        </span>
                      </div>
                    ))}
                  </div>
                  <div className="mt-4 text-center">
                    <p className="text-sm text-gray-600">
                      Healing score progression over time
                    </p>
                  </div>
                </div>
              ) : (
                <div className="text-center py-8">
                  <Activity className="h-12 w-12 text-gray-300 mx-auto mb-3" />
                  <p className="text-gray-500">
                    No healing data available yet
                  </p>
                </div>
              )}
            </div>
          </div>
          
          {/* Right Column - Doctor Chat */}
          <div className="lg:col-span-1">
            <div className="bg-white rounded-xl shadow-lg h-full flex flex-col">
              <div className="p-4 border-b">
                <div className="flex items-center justify-between">
                  <div className="flex items-center gap-3">
                    <div className="bg-blue-100 w-10 h-10 rounded-full flex items-center justify-center">
                      <User className="h-5 w-5 text-blue-600" />
                    </div>
                    <div>
                      <h2 className="font-bold text-gray-800">Virtual Doctor</h2>
                      <p className="text-sm text-gray-500">Online now</p>
                    </div>
                  </div>
                  <button 
                    onClick={() => setIsChatOpen(false)}
                    className="lg:hidden text-gray-500 hover:text-gray-700"
                  >
                    <X className="h-5 w-5" />
                  </button>
                </div>
              </div>
              
              <div 
                ref={chatContainerRef}
                className="flex-1 overflow-y-auto p-4 space-y-4"
              >
                {chatMessages.length > 0 ? (
                  chatMessages.map((message) => (
                    <div 
                      key={message.id} 
                      className={flex ${message.sender === 'user' ? 'justify-end' : 'justify-start'}}
                    >
                      <div 
                        className={max-w-[80%] rounded-2xl px-4 py-2 ${message.sender === 'user' ? 'bg-blue-500 text-white rounded-br-none' : 'bg-gray-100 text-gray-800 rounded-bl-none'}}
                      >
                        {message.sender === 'doctor' && currentDoctor && (
                          <div className="flex items-center gap-2 mb-1">
                            <div className="bg-blue-100 w-6 h-6 rounded-full flex items-center justify-center">
                              <span className="text-xs font-bold text-blue-700">{currentDoctor.avatar}</span>
                            </div>
                            <span className="text-xs font-medium">{currentDoctor.name}</span>
                          </div>
                        )}
                        <p>{message.text}</p>
                        <p className={text-xs mt-1 ${message.sender === 'user' ? 'text-blue-100' : 'text-gray-500'}}>
                          {message.timestamp.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })}
                        </p>
                      </div>
                    </div>
                  ))
                ) : (
                  <div className="text-center py-10">
                    <MessageCircle className="h-12 w-12 text-gray-300 mx-auto mb-3" />
                    <p className="text-gray-500">
                      Start a conversation with your virtual doctor
                    </p>
                  </div>
                )}
              </div>
              
              <div className="p-4 border-t">
                <div className="flex gap-2">
                  <input
                    type="text"
                    value={newMessage}
                    onChange={(e) => setNewMessage(e.target.value)}
                    onKeyPress={handleKeyPress}
                    placeholder="Type your message..."
                    className="flex-1 border border-gray-300 rounded-full px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                  />
                  <button
                    onClick={handleSendMessage}
                    disabled={!newMessage.trim()}
                    className="bg-blue-600 text-white rounded-full p-2 hover:bg-blue-700 disabled:opacity-50"
                  >
                    <Send className="h-5 w-5" />
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        {/* Footer */}
        <footer className="mt-8 text-center text-sm text-gray-500">
          <p>AI Wound Healing Monitor • For medical use only • Consult your healthcare provider</p>
        </footer>
      </div>
      
      {/* Mobile Chat Toggle */}
      {analysisResults && !isChatOpen && (
        <button
          onClick={() => setIsChatOpen(true)}
          className="fixed bottom-6 right-6 bg-blue-600 text-white rounded-full p-4 shadow-lg hover:bg-blue-700 lg:hidden"
        >
          <MessageCircle className="h-6 w-6" />
        </button>
      )}
    </div>
  );
};

export default App;
