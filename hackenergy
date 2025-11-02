// GeoMint - AI-Powered Attendance and Payroll Management
// Theme: Deep Forest Green (#004D40) and Warm Bronze (#C27C36)
// This is a single-file, fully responsive React application for the hackathon.

import React, { useState, useEffect, useCallback, useMemo } from 'react';
import {
  Home, Users, CheckSquare, Briefcase, BarChart, Settings, Award, MapPin,
  Clock, Bell, User, Cpu, Zap, LogOut, ArrowLeft, Loader, TrendingUp, DollarSign, PieChart, Clipboard, Sun, Moon
} from 'lucide-react';
import {
  BarChart as RechartsBarChart, Bar, PieChart as RechartsPieChart, Pie, Cell, LineChart as RechartsLineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer, RadialBarChart, RadialBar
} from 'recharts';

// --- Color and Style Definitions ---
const COLORS = {
  primary: '#004D40', // Deep Forest Green
  accent: '#C27C36',  // Warm Bronze
  secondary: '#69A197', // Muted Teal/Sage
  text: '#444B59',    // Charcoal
  background: '#F7FDFD', // Off-White/Mint Cream
  error: '#EF4444',
  success: '#10B981',
};

// --- Mock Data ---

const MOCK_EMPLOYEES = [
  { id: 'E-101A', name: 'Alice Johnson', trustIndex: 98, status: 'In-Zone', location: '40.7128, -74.0060', avatar: 'https://placehold.co/40x40/004D40/FFFFFF?text=AJ' },
  { id: 'E-231F', name: 'Mark Wilson', trustIndex: 85, status: 'Out-of-Zone', location: '40.7306, -73.9352', avatar: 'https://placehold.co/40x40/C27C36/FFFFFF?text=MW' },
  { id: 'E-559R', name: 'David Lee', trustIndex: 92, status: 'Verified', location: '40.7580, -73.9855', avatar: 'https://placehold.co/40x40/69A197/FFFFFF?text=DL' },
];

const MOCK_ACTIVITY = [
  { employee: 'Alice Johnson', photo: 'AJ', location: 'Site 1', clockIn: '08:00 AM', clockOut: '05:00 PM', status: 'Verified', trustIndex: 98, hash: '0x8c7e...3d2f' },
  { employee: 'Mark Wilson', photo: 'MW', location: 'Home', clockIn: '08:01 AM', clockOut: 'N/A', status: 'Out-of-Zone', trustIndex: 85, hash: '0x3a5b...1f7c' },
  { employee: 'David Lee', photo: 'DL', location: 'Site 3', clockIn: '09:00 AM', clockOut: '06:00 PM', status: 'Verified', trustIndex: 92, hash: '0x1b2d...9h4a' },
  { employee: 'Sarah Chen', photo: 'SC', location: 'Site 2', clockIn: '08:30 AM', clockOut: '04:30 PM', status: 'Verified', trustIndex: 95, hash: '0x7f8g...2k6p' },
];

const MOCK_HOURS_DATA = [
  { name: 'Alice', hours: 45, pv: 2400, amt: 2400 },
  { name: 'Mark', hours: 41, pv: 1398, amt: 2210 },
  { name: 'David', hours: 48, pv: 9800, amt: 2290 },
  { name: 'Sarah', hours: 38, pv: 3908, amt: 2000 },
];

const MOCK_ATTENDANCE_PIE = [
  { name: 'Verified', value: 320, color: COLORS.success },
  { name: 'Out-of-Zone', value: 30, color: COLORS.error },
  { name: 'Manual Review', value: 50, color: COLORS.accent },
];

const MOCK_TRENDS_DATA = [
  { name: 'Mon', hours: 8.5 },
  { name: 'Tue', hours: 8.2 },
  { name: 'Wed', hours: 8.6 },
  { name: 'Thu', hours: 9.1 },
  { name: 'Fri', hours: 8.0 },
];

// --- Utility Components ---

const Card = ({ children, className = '' }) => (
  <div className={`bg-white shadow-xl rounded-[20px] p-6 transition duration-300 hover:shadow-2xl ${className}`}>
    {children}
  </div>
);

const Button = ({ children, onClick, variant = 'primary', className = '', disabled = false }) => {
  const baseStyle = 'py-3 px-6 rounded-xl font-semibold transition-all duration-300 shadow-md';
  let variantStyle = '';
  if (variant === 'primary') {
    variantStyle = `bg-[${COLORS.primary}] text-white hover:bg-[${COLORS.secondary}] disabled:opacity-50`;
  } else if (variant === 'accent') {
    variantStyle = `bg-[${COLORS.accent}] text-[${COLORS.primary}] hover:bg-opacity-90 disabled:opacity-50`;
  } else if (variant === 'ghost') {
    variantStyle = `bg-transparent text-[${COLORS.primary}] hover:bg-gray-100 disabled:opacity-50`;
  } else if (variant === 'danger') {
    variantStyle = `bg-[${COLORS.error}] text-white hover:bg-red-700 disabled:opacity-50`;
  }

  return (
    <button
      className={`${baseStyle} ${variantStyle} ${className} active:scale-[0.98]`}
      onClick={onClick}
      disabled={disabled}
    >
      {children}
    </button>
  );
};

// Custom Toast Component for system messages
const Toast = ({ show, message, type }) => {
  if (!show) return null;

  const color = type === 'success' ? `bg-[${COLORS.success}]` : `bg-[${COLORS.error}]`;
  const icon = type === 'success' ? <CheckSquare className="w-5 h-5" /> : <Bell className="w-5 h-5" />;

  return (
    <div
      className={`fixed bottom-4 right-4 p-4 rounded-xl text-white shadow-lg flex items-center space-x-3 z-50 animate-fadeIn ${color}`}
    >
      {icon}
      <span>{message}</span>
    </div>
  );
};

// --- Modal Component ---

const Modal = ({ show, onClose, title, children }) => {
  if (!show) return null;

  return (
    <div className="fixed inset-0 z-[100] flex items-center justify-center p-4 bg-black bg-opacity-50 backdrop-blur-sm transition-opacity duration-300 animate-fadeIn">
      <Card className="max-w-md w-full animate-slideUp">
        <div className="flex justify-between items-center border-b pb-3 mb-4 border-gray-200">
          <h2 className={`text-xl font-bold font-montserrat text-[${COLORS.primary}]`}>{title}</h2>
          <Button onClick={onClose} variant="ghost" className="p-1 rounded-full text-gray-500 hover:text-gray-700">
            &times;
          </Button>
        </div>
        <div>{children}</div>
      </Card>
    </div>
  );
};

// --- Feature-Specific Components ---

// Map Placeholder for dependency resolution
const PlaceholderMap = ({ height = 'h-64' }) => (
  <div className={`bg-gray-200 rounded-xl flex items-center justify-center text-center ${height} text-gray-500`}>
    <MapPin className="w-6 h-6 mr-2" />
    Map Visualization Placeholder
    <p className='absolute bottom-2 text-xs'>(*Map functionality removed for stable compilation)</p>
  </div>
);

// Trust Index Gauge (Innovation Mode)
const TrustIndexGauge = () => {
  const trustData = [{
    name: 'Trust Index',
    value: 92,
    fill: COLORS.accent,
  }];

  return (
    <ResponsiveContainer width="100%" height={200}>
      <RadialBarChart
        innerRadius="70%"
        outerRadius="90%"
        data={trustData}
        startAngle={270}
        endAngle={-90}
      >
        <RadialBar dataKey="value" cornerRadius={10} background={{ fill: '#eee' }} />
        <text
          x="50%"
          y="50%"
          textAnchor="middle"
          dominantBaseline="middle"
          className={`text-3xl font-bold fill-[${COLORS.accent}]`}
        >
          92%
        </text>
        <Tooltip />
      </RadialBarChart>
    </ResponsiveContainer>
  );
};

// Rewards Modal Content
const RewardCenterModalContent = () => (
  <div className="text-center p-4">
    <Award className={`w-16 h-16 mx-auto text-[${COLORS.accent}] animate-bounce`} />
    <h3 className={`text-2xl font-bold mt-4 font-montserrat text-[${COLORS.primary}]`}>Achievement Unlocked!</h3>
    <p className="text-gray-600 mt-2">You earned the **Early Bird** badge for clocking in before 8:15 AM for five consecutive days.</p>
    <div className="mt-4 p-4 bg-gray-100 rounded-lg">
      <p className="font-semibold text-sm">Trust Score Progress</p>
      <div className="w-full bg-gray-200 rounded-full h-2.5 mt-2">
        <div className={`h-2.5 rounded-full`} style={{ width: '95%', backgroundColor: COLORS.success }}></div>
      </div>
      <p className="text-xs mt-1 text-gray-500">Next reward at 100% Trust Index!</p>
    </div>
    <Button onClick={() => {}} variant="primary" className="mt-6 w-full">View All Badges</Button>
  </div>
);

// Anomaly Alerts Modal Content
const AnomalyAlertsModalContent = ({ onClose }) => (
  <div>
    <h3 className={`text-xl font-bold font-montserrat text-[${COLORS.error}] mb-3`}>Open Anomaly Alerts (2)</h3>
    <div className="space-y-4">
      <Card className="border border-red-200 p-3">
        <p className="font-semibold text-[${COLORS.primary}]">Out-of-Zone Clock-In</p>
        <p className={`text-sm text-[${COLORS.error}] mt-1`}>
          **Mark Wilson (E-231F)** clocked in at 08:01 AM from an unauthorized location (Home). Geofence check failed.
        </p>
        <Button variant="danger" className="mt-2 text-sm px-3 py-1">Flag for HR Review</Button>
      </Card>
      <Card className="border border-yellow-200 p-3">
        <p className="font-semibold text-[${COLORS.primary}]">Potential Buddy Punching</p>
        <p className={`text-sm text-[${COLORS.accent}] mt-1`}>
          **David Lee (E-559R)** clocked in 3 seconds after Alice Johnson at the same terminal. Low Liveness score.
        </p>
        <Button variant="accent" className="mt-2 text-sm px-3 py-1">Request Liveness Audit</Button>
      </Card>
    </div>
    <Button onClick={onClose} variant="ghost" className="mt-4 w-full">Close</Button>
  </div>
);

// --- Page Components ---

const LandingPage = ({ setView }) => {
  const [isAnimating, setIsAnimating] = useState(false);

  const handleViewChange = (newView) => {
    setIsAnimating(true);
    setTimeout(() => {
      setView(newView);
      setIsAnimating(false);
    }, 500);
  };

  // Grid background pattern
  const GridPattern = () => (
    <div
      className="absolute inset-0 opacity-10"
      style={{
        backgroundImage: `linear-gradient(to right, ${COLORS.primary} 1px, transparent 1px), linear-gradient(to bottom, ${COLORS.primary} 1px, transparent 1px)`,
        backgroundSize: '40px 40px',
      }}
    ></div>
  );

  return (
    <div
      className={`min-h-screen flex items-center justify-center p-4 bg-[${COLORS.primary}] transition-opacity duration-500 ${isAnimating ? 'opacity-0' : 'opacity-100'}`}
    >
      <GridPattern />
      <Card className="max-w-4xl w-full text-center p-10 relative z-10">
        <h1 className={`text-5xl font-extrabold font-montserrat mb-4 text-[${COLORS.primary}]`}>
          GeoMint <span className={`text-[${COLORS.accent}]`}>—</span> Minting Trust in Workplaces
        </h1>
        <p className={`text-xl mb-10 text-[${COLORS.text}]`}>
          AI-verified attendance and payroll for field teams.
        </p>

        <div className="grid md:grid-cols-2 gap-6">
          <Button onClick={() => handleViewChange('employee')} variant="accent" className="text-xl py-4 shadow-lg shadow-yellow-200">
            <User className="inline mr-2 w-6 h-6" /> Try Employee View
          </Button>
          <Button onClick={() => handleViewChange('admin')} variant="primary" className="text-xl py-4 shadow-lg shadow-green-200">
            <Briefcase className="inline mr-2 w-6 h-6" /> Open HR Dashboard
          </Button>
        </div>

        <div className="mt-12">
          <h2 className={`text-2xl font-bold font-montserrat mb-6 text-[${COLORS.primary}]`}>Key Features</h2>
          <div className="grid grid-cols-2 md:grid-cols-4 gap-4 text-sm">
            <FeaturePill Icon={Cpu} title="AI Verification" />
            <FeaturePill Icon={MapPin} title="Adaptive Geofencing" />
            <FeaturePill Icon={DollarSign} title="Predictive Payroll" />
            <FeaturePill Icon={Award} title="Gamified Rewards" />
          </div>
        </div>
      </Card>
    </div>
  );
};

const FeaturePill = ({ Icon, title }) => (
  <div className={`p-3 border-2 border-[${COLORS.secondary}] rounded-full flex items-center justify-center space-x-2 text-[${COLORS.text}] transition duration-300 hover:bg-gray-50`}>
    <Icon className={`w-5 h-5 text-[${COLORS.primary}]`} />
    <span className="font-semibold">{title}</span>
  </div>
);


// --- Employee View Components ---

const EmployeeViewModal = ({ setView }) => {
  const [clockedIn, setClockedIn] = useState(false);
  const [status, setStatus] = useState('Ready to Clock In');
  const [livenessStatus, setLivenessStatus] = useState('inactive'); // inactive, scanning, confirmed, failed
  const [gpsStatus, setGpsStatus] = useState('Location Status: Verifying...');
  const [showRewardModal, setShowRewardModal] = useState(false);

  // Time update useEffect
  const [time, setTime] = useState(new Date());
  useEffect(() => {
    const timer = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(timer);
  }, []);

  const handleClockAction = () => {
    if (clockedIn) {
      // Clock Out
      setStatus('Clocking Out...');
      setTimeout(() => {
        setClockedIn(false);
        setStatus('Clocked Out. Have a great evening!');
        // setToast({ show: true, message: 'Clock Out Successful!', type: 'success' });
      }, 1500);
      return;
    }

    // --- Clock In Logic ---
    setStatus('Initiating AI & GPS Verification...');
    setLivenessStatus('scanning');

    setTimeout(() => {
      // Simulate GPS Check (Random failure 10% of the time)
      const isOutofZone = Math.random() < 0.1;

      if (isOutofZone) {
        setGpsStatus('Out of Work Zone! Clock-In Denied.');
        setLivenessStatus('failed');
        setStatus('Verification Failed.');
        // setToast({ show: true, message: 'Clock-In Failed: Out of Zone', type: 'error' });
      } else {
        // GPS Confirmed, Proceed to Liveness Check (Simulated gesture)
        setGpsStatus('Location Verified: Site A (In-Zone)');
        setLivenessStatus('confirmed');
        setStatus('AI Liveness Check Complete.');

        setTimeout(() => {
          setClockedIn(true);
          setStatus('Clocked In Successfully!');
          setShowRewardModal(true); // Trigger reward
          // setToast({ show: true, message: 'Attendance Verified – AI and GPS Confirmed', type: 'success' });
        }, 1500); // Wait for the "gesture"
      }
    }, 2500); // Total verification time
  };

  const getLivenessUI = () => {
    if (livenessStatus === 'scanning') {
      return (
        <div className="flex flex-col items-center justify-center h-full bg-black/80 text-white">
          <Loader className={`w-12 h-12 animate-spin text-[${COLORS.accent}]`} />
          <p className="mt-3 font-semibold">AI Liveness Check: Nod Your Head Now</p>
          <p className="text-sm text-gray-400">(Simulating movement gesture verification)</p>
        </div>
      );
    }
    if (livenessStatus === 'confirmed') {
      return (
        <div className="flex flex-col items-center justify-center h-full bg-[${COLORS.success}]/90 text-white">
          <CheckSquare className="w-12 h-12" />
          <p className="mt-3 font-bold">Liveness Confirmed!</p>
        </div>
      );
    }
    if (livenessStatus === 'failed') {
      return (
        <div className="flex flex-col items-center justify-center h-full bg-[${COLORS.error}]/90 text-white">
          <Zap className="w-12 h-12" />
          <p className="mt-3 font-bold">Liveness Failed / Tamper Detected</p>
        </div>
      );
    }
    // inactive
    return (
      <div className="flex items-center justify-center h-full bg-gray-900 text-gray-500">
        <User className="w-10 h-10 mr-2" />
        Webcam Inactive (Ready for AI Scan)
      </div>
    );
  };

  return (
    <div className={`min-h-screen flex items-center justify-center p-4 bg-[${COLORS.background}]`}>
      <Card className="w-full max-w-sm p-0 overflow-hidden shadow-2xl relative">
        <div className={`p-4 flex items-center justify-between text-white bg-[${COLORS.primary}]`}>
          <button onClick={() => setView('landing')} className="text-white">
            <ArrowLeft className="w-5 h-5" />
          </button>
          <span className="font-semibold text-lg">GeoMint Field App</span>
          <Bell className="w-5 h-5" />
        </div>

        {/* --- Mobile Screen Content --- */}
        <div className="p-5">
          {/* Header Info */}
          <div className="flex items-center space-x-4 mb-6">
            <img
              src="https://placehold.co/60x60/C27C36/FFFFFF?text=AJ"
              alt="Profile"
              className="rounded-full w-14 h-14 object-cover border-4 border-gray-100 shadow-md"
            />
            <div>
              <h2 className={`text-xl font-bold font-montserrat text-[${COLORS.primary}]`}>Alice Johnson</h2>
              <p className="text-sm text-gray-500">ID: E-101A | Trust Index: 98%</p>
            </div>
          </div>

          {/* Time and Status */}
          <Card className="mb-6 text-center p-4 bg-gray-50">
            <p className={`text-4xl font-extrabold font-montserrat text-[${COLORS.primary}]`}>
              {time.toLocaleTimeString()}
            </p>
            <p className="text-sm text-gray-500">{time.toDateString()}</p>
          </Card>

          {/* Webcam Simulation */}
          <div className="h-40 w-full rounded-xl overflow-hidden mb-4 border-4 border-gray-100 shadow-inner">
            {getLivenessUI()}
          </div>

          {/* GPS Status */}
          <p className={`text-center font-semibold text-sm mb-6 ${gpsStatus.includes('Verified') ? `text-[${COLORS.success}]` : `text-[${COLORS.error}]`}`}>
            <MapPin className="inline w-4 h-4 mr-1" />
            {gpsStatus}
          </p>

          {/* Action Button */}
          <Button
            onClick={handleClockAction}
            variant={clockedIn ? 'danger' : 'accent'}
            className="w-full text-lg py-4 shadow-lg"
            disabled={livenessStatus === 'scanning'}
          >
            {livenessStatus === 'scanning' ? (
              <Loader className="w-5 h-5 mr-2 animate-spin inline" />
            ) : clockedIn ? (
              <LogOut className="inline w-5 h-5 mr-2" />
            ) : (
              <Clock className="inline w-5 h-5 mr-2" />
            )}
            {livenessStatus === 'scanning' ? 'Verifying...' : (clockedIn ? 'Clock Out' : 'Clock In')}
          </Button>

          {/* Current Status */}
          <p className={`text-center mt-4 text-sm font-medium ${status.includes('Successfully') ? `text-[${COLORS.success}]` : 'text-gray-500'}`}>
            {status}
          </p>

          {/* Offline Mode Banner */}
          <div className="mt-8 p-3 text-center text-xs bg-gray-200 text-gray-600 rounded-lg">
            <Zap className="w-3 h-3 inline mr-1" /> You are offline. Data will sync automatically.
          </div>
        </div>
      </Card>
      <Modal show={showRewardModal} onClose={() => setShowRewardModal(false)} title="GeoMint Rewards">
        <RewardCenterModalContent />
      </Modal>
    </div>
  );
};


// --- Admin Dashboard Components ---

const Sidebar = ({ page, setPage, isDarkMode, setIsDarkMode }) => {
  const NavItem = ({ icon: Icon, name, targetPage }) => (
    <div
      className={`flex items-center p-3 rounded-xl cursor-pointer transition-all duration-200 
        ${page === targetPage
          ? `bg-white/10 text-[${COLORS.accent}] font-bold border-l-4 border-[${COLORS.accent}]`
          : 'text-white/80 hover:bg-white/5'
        }`}
      onClick={() => setPage(targetPage)}
    >
      <Icon className="w-5 h-5 mr-4" />
      <span>{name}</span>
    </div>
  );

  return (
    <div className={`w-64 min-h-screen p-6 flex flex-col fixed inset-y-0 z-20 transition-colors duration-500 
      ${isDarkMode ? 'bg-[#1F2937]' : `bg-[${COLORS.primary}]`}`}>
      <div className={`text-3xl font-extrabold font-montserrat mb-10 border-b pb-4 ${isDarkMode ? 'text-white' : `text-white border-white/20`}`}>
        Geo<span className={`text-[${COLORS.accent}]`}>Mint</span>
      </div>

      <nav className="space-y-2 flex-grow">
        <NavItem icon={Home} name="Dashboard" targetPage="dashboard" />
        <NavItem icon={Users} name="Employees" targetPage="employees" />
        <NavItem icon={CheckSquare} name="Attendance" targetPage="attendance" />
        <NavItem icon={DollarSign} name="Payroll" targetPage="payroll" />
        <NavItem icon={BarChart} name="Reports & Analytics" targetPage="reports" />
        <div className='h-4'></div>
        <p className={`text-sm font-semibold pt-4 border-t ${isDarkMode ? 'text-gray-400 border-gray-700' : 'text-white/50 border-white/20'}`}>Innovation</p>
        <NavItem icon={MapPin} name="Adaptive Geofencing" targetPage="geofencing" />
        <NavItem icon={TrendingUp} name="Predictive Payroll" targetPage="predictive-payroll" />
        <NavItem icon={Award} name="Gamified Rewards" targetPage="rewards" />
      </nav>

      <div className="pt-6 border-t border-white/20 mt-6">
        <NavItem icon={Settings} name="Settings" targetPage="settings" />
        <NavItem icon={LogOut} name="About GeoMint" targetPage="about" />
        <div className={`mt-4 flex justify-between items-center p-2 rounded-xl cursor-pointer text-white/80 ${isDarkMode ? 'bg-gray-800' : 'bg-white/5'}`}>
          <span className="font-semibold text-sm">Dark Mode</span>
          <button
            onClick={() => setIsDarkMode(!isDarkMode)}
            className={`p-1 rounded-full transition duration-300 ${isDarkMode ? 'bg-gray-600' : `bg-[${COLORS.accent}]`}`}
            aria-label="Toggle dark mode"
          >
            {isDarkMode ? <Moon className="w-5 h-5 text-white" /> : <Sun className="w-5 h-5 text-[${COLORS.primary}]" />}
          </button>
        </div>
      </div>
    </div>
  );
};

// --- Dashboards and Pages ---

const DashboardContent = ({ setModalContent, setShowModal, isInnovationMode, isDarkMode }) => {
  const getStatusColor = (status) => {
    if (status === 'Verified') return COLORS.success;
    if (status === 'Out-of-Zone') return COLORS.error;
    return COLORS.accent;
  };

  const SummaryCard = ({ title, value, icon: Icon, color }) => (
    <Card className={`p-5 flex items-center justify-between border-b-4 ${isDarkMode ? 'bg-[#2D3748] text-white' : 'bg-white text-gray-800'}`}
      style={{ borderBottomColor: color }}>
      <div>
        <p className="text-sm font-medium text-gray-500">{title}</p>
        <h3 className={`text-3xl font-extrabold font-montserrat ${isDarkMode ? 'text-white' : 'text-[${COLORS.primary}]'}`}>{value}</h3>
      </div>
      <Icon className="w-8 h-8 opacity-20" style={{ color: color }} />
    </Card>
  );

  return (
    <div className="space-y-8">
      {/* 1. Summary Cards */}
      <div className="grid grid-cols-1 md:grid-cols-4 gap-6">
        <SummaryCard title="Verified Check-ins (Today)" value="289" icon={CheckSquare} color={COLORS.success} />
        <SummaryCard title="Out-of-Zone Alerts" value="4" icon={Zap} color={COLORS.error} />
        <SummaryCard title="Payroll Estimate (Weekly)" value="$35,200" icon={DollarSign} color={COLORS.primary} />
        <SummaryCard title="Average Trust Index" value="92.1%" icon={TrendingUp} color={COLORS.accent} />
      </div>

      {/* 2. Innovation Showcase */}
      {isInnovationMode && (
        <InnovationShowcase isDarkMode={isDarkMode} />
      )}

      {/* 3. Core Analytics (Charts & Map) */}
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        {/* Live Map (Placeholder) */}
        <Card className="lg:col-span-2 p-4">
          <h2 className={`text-xl font-bold font-montserrat mb-4 ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>Live Employee Locations (Simulated)</h2>
          <PlaceholderMap height="h-80" />
          <div className="mt-3 flex justify-between">
            <p className="text-sm text-gray-500">Geofence Status: Adaptive & Active</p>
            <Button variant="ghost" className="text-xs py-1" onClick={() => {
              setModalContent(<AnomalyAlertsModalContent onClose={() => setShowModal(false)} />);
              setShowModal(true);
            }}>
              View Anomaly Alerts
            </Button>
          </div>
        </Card>

        {/* Attendance Breakdown Pie Chart */}
        <Card className={`p-4 ${isDarkMode ? 'bg-[#2D3748] text-white' : 'bg-white text-gray-800'}`}>
          <h2 className={`text-xl font-bold font-montserrat mb-4 ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>Attendance Breakdown</h2>
          <ResponsiveContainer width="100%" height={250}>
            <RechartsPieChart>
              <Pie
                data={MOCK_ATTENDANCE_PIE}
                dataKey="value"
                nameKey="name"
                cx="50%"
                cy="50%"
                outerRadius={100}
                fill="#8884d8"
                labelLine={false}
                label={({ name, percent }) => `${name}: ${(percent * 100).toFixed(0)}%`}
              >
                {MOCK_ATTENDANCE_PIE.map((entry, index) => (
                  <Cell key={`cell-${index}`} fill={entry.color} />
                ))}
              </Pie>
              <Tooltip />
              <Legend />
            </RechartsPieChart>
          </ResponsiveContainer>
        </Card>
      </div>

      {/* 4. Recent Activity Table */}
      <Card>
        <h2 className={`text-xl font-bold font-montserrat mb-4 ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>Recent Verified Activity</h2>
        <div className="overflow-x-auto">
          <table className="min-w-full divide-y divide-gray-200">
            <thead className={`bg-gray-50 ${isDarkMode ? 'bg-gray-700' : ''}`}>
              <tr>
                {['Employee', 'Location', 'Clock-in Time', 'Status', 'Trust Index', 'Trust Chain Hash'].map(header => (
                  <th key={header} className={`px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider ${isDarkMode ? 'text-gray-300' : ''}`}>
                    {header}
                  </th>
                ))}
              </tr>
            </thead>
            <tbody className={`divide-y divide-gray-200 ${isDarkMode ? 'divide-gray-700' : ''}`}>
              {MOCK_ACTIVITY.map((activity, index) => (
                <tr key={index} className={`transition-colors duration-200 ${isDarkMode ? 'hover:bg-gray-800' : 'hover:bg-gray-50'}`}>
                  <td className={`px-6 py-4 whitespace-nowrap text-sm font-medium ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>
                    {activity.employee}
                  </td>
                  <td className={`px-6 py-4 whitespace-nowrap text-sm ${isDarkMode ? 'text-gray-300' : 'text-gray-500'}`}>
                    {activity.location}
                  </td>
                  <td className={`px-6 py-4 whitespace-nowrap text-sm ${isDarkMode ? 'text-gray-300' : 'text-gray-500'}`}>
                    {activity.clockIn}
                  </td>
                  <td className={`px-6 py-4 whitespace-nowrap text-sm font-semibold`} style={{ color: getStatusColor(activity.status) }}>
                    {activity.status}
                  </td>
                  <td className={`px-6 py-4 whitespace-nowrap text-sm ${isDarkMode ? 'text-gray-300' : 'text-gray-500'}`}>
                    {activity.trustIndex}%
                  </td>
                  <td className={`px-6 py-4 whitespace-nowrap text-sm font-mono ${isDarkMode ? 'text-gray-300' : 'text-gray-500'}`}>
                    {activity.hash}
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </Card>
    </div>
  );
};

const InnovationShowcase = ({ isDarkMode }) => (
  <Card className={`${isDarkMode ? 'bg-[#2D3748] text-white' : ''}`}>
    <h2 className={`text-2xl font-bold font-montserrat mb-4 text-[${COLORS.accent}]`}>Innovation Showcase: AI-Powered Trust</h2>
    <div className="grid grid-cols-1 md:grid-cols-3 gap-6">

      {/* AI Liveness Detection */}
      <Card className={`p-4 border-l-4 border-[${COLORS.success}] ${isDarkMode ? 'bg-gray-700' : 'bg-gray-50'}`}>
        <div className="flex items-center space-x-3 mb-2">
          <Cpu className={`w-6 h-6 text-[${COLORS.success}]`} />
          <h3 className="font-bold text-lg">AI Liveness Detection</h3>
        </div>
        <p className="text-sm text-gray-600">Simulates real-time face verification and movement gesture analysis to prevent photo/video-based buddy punching.</p>
        <div className="mt-3 flex items-center justify-between text-xs font-semibold">
          <span className={`text-[${COLORS.success}]`}>Success Rate: 99.8%</span>
          <Loader className='w-4 h-4 text-gray-400 animate-pulse' />
        </div>
      </Card>

      {/* Adaptive Geofencing */}
      <Card className={`p-4 border-l-4 border-[${COLORS.secondary}] ${isDarkMode ? 'bg-gray-700' : 'bg-gray-50'}`}>
        <div className="flex items-center space-x-3 mb-2">
          <MapPin className={`w-6 h-6 text-[${COLORS.secondary}]`} />
          <h3 className="font-bold text-lg">Adaptive Geofencing</h3>
        </div>
        <p className="text-sm text-gray-600">Dynamically adjusts zone size based on GPS signal variance, reducing false "out-of-zone" alerts by 40%.</p>
        <div className="mt-3 flex items-center justify-between text-xs font-semibold">
          <span className={`text-[${COLORS.secondary}]`}>Alert Reduction: 40%</span>
          <Zap className='w-4 h-4 text-gray-400' />
        </div>
      </Card>

      {/* Trust Index Meter */}
      <Card className={`p-4 border-l-4 border-[${COLORS.accent}] ${isDarkMode ? 'bg-gray-700' : 'bg-gray-50'}`}>
        <div className="flex items-center space-x-3 mb-2">
          <TrendingUp className={`w-6 h-6 text-[${COLORS.accent}]`} />
          <h3 className="font-bold text-lg">Employee Trust Index</h3>
        </div>
        <p className="text-sm text-gray-600">Gamified score based on attendance history, compliance, and location accuracy. Drives positive behavior.</p>
        <TrustIndexGauge />
      </Card>
    </div>
  </Card>
);

const ReportsPage = ({ isDarkMode }) => (
  <div className="space-y-8">
    <h1 className={`text-3xl font-extrabold font-montserrat ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>Reports & Analytics</h1>

    {/* Filters and Actions */}
    <Card className={`flex flex-col md:flex-row gap-4 ${isDarkMode ? 'bg-[#2D3748] text-white' : ''}`}>
      <select className={`input-field flex-grow p-2 rounded-lg border ${isDarkMode ? 'bg-gray-700 border-gray-600 text-white' : 'border-gray-300'}`}>
        <option>All Employees</option>
        <option>Field Team A</option>
        <option>HQ Staff</option>
      </select>
      <input type="date" className={`input-field flex-grow p-2 rounded-lg border ${isDarkMode ? 'bg-gray-700 border-gray-600 text-white' : 'border-gray-300'}`} defaultValue="2024-10-01" />
      <input type="date" className={`input-field flex-grow p-2 rounded-lg border ${isDarkMode ? 'bg-gray-700 border-gray-600 text-white' : 'border-gray-300'}`} defaultValue="2024-10-31" />
      <Button variant="accent">Filter</Button>
      <Button variant="primary">Export CSV/PDF</Button>
    </Card>

    {/* Payroll Summary and Trends */}
    <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
      {/* Payroll Summary */}
      <Card className={`p-5 border-l-4 border-[${COLORS.primary}] ${isDarkMode ? 'bg-[#2D3748] text-white' : ''}`}>
        <h2 className={`text-xl font-bold font-montserrat mb-4 ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>Payroll Summary (Oct 2024)</h2>
        <div className="space-y-2 text-sm">
          <div className="flex justify-between border-b pb-1"><span>Total Billable Hours:</span> <span className="font-semibold text-lg">1,280 hrs</span></div>
          <div className="flex justify-between border-b pb-1"><span>Total Overtime:</span> <span className={`font-semibold text-lg text-[${COLORS.accent}]`}>115 hrs</span></div>
          <div className="flex justify-between border-b pb-1"><span>Anomalies Flagged:</span> <span className={`font-semibold text-lg text-[${COLORS.error}]`}>12</span></div>
          <div className="flex justify-between pt-2">
            <span className="text-lg font-bold">Total Estimated Payout:</span>
            <span className={`text-2xl font-extrabold text-[${COLORS.primary}]`} style={{ color: COLORS.accent }}>$48,500</span>
          </div>
        </div>
      </Card>

      {/* Attendance Trends */}
      <Card className={`p-5 ${isDarkMode ? 'bg-[#2D3748] text-white' : ''}`}>
        <h2 className={`text-xl font-bold font-montserrat mb-4 ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>Daily Average Hours Trend</h2>
        <ResponsiveContainer width="100%" height={250}>
          <RechartsLineChart data={MOCK_TRENDS_DATA} margin={{ top: 5, right: 30, left: 20, bottom: 5 }}>
            <CartesianGrid strokeDasharray="3 3" stroke={isDarkMode ? '#4A5568' : '#e0e0e0'} />
            <XAxis dataKey="name" stroke={isDarkMode ? '#CBD5E0' : COLORS.text} />
            <YAxis unit="hrs" stroke={isDarkMode ? '#CBD5E0' : COLORS.text} />
            <Tooltip contentStyle={{ backgroundColor: isDarkMode ? '#1F2937' : 'white', border: 'none', borderRadius: '8px' }} />
            <Line type="monotone" dataKey="hours" stroke={COLORS.primary} strokeWidth={3} activeDot={{ r: 8, fill: COLORS.accent }} />
          </RechartsLineChart>
        </ResponsiveContainer>
      </Card>
    </div>

    {/* Total Hours Worked Bar Chart */}
    <Card className={`p-5 ${isDarkMode ? 'bg-[#2D3748] text-white' : ''}`}>
      <h2 className={`text-xl font-bold font-montserrat mb-4 ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>Total Hours Worked by Employee (Last 7 Days)</h2>
      <ResponsiveContainer width="100%" height={300}>
        <RechartsBarChart data={MOCK_HOURS_DATA} margin={{ top: 20, right: 30, left: 20, bottom: 5 }}>
          <CartesianGrid strokeDasharray="3 3" stroke={isDarkMode ? '#4A5568' : '#f0f0f0'} />
          <XAxis dataKey="name" stroke={isDarkMode ? '#CBD5E0' : COLORS.text} />
          <YAxis unit="hrs" stroke={isDarkMode ? '#CBD5E0' : COLORS.text} />
          <Tooltip contentStyle={{ backgroundColor: isDarkMode ? '#1F2937' : 'white', border: 'none', borderRadius: '8px' }} />
          <Legend />
          <Bar dataKey="hours" fill={COLORS.primary} radius={[10, 10, 0, 0]} />
        </RechartsBarChart>
      </ResponsiveContainer>
    </Card>
  </div>
);

const AboutPage = ({ isDarkMode }) => (
  <div className="space-y-8">
    <h1 className={`text-3xl font-extrabold font-montserrat ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>About GeoMint</h1>

    <Card className={`p-6 ${isDarkMode ? 'bg-[#2D3748] text-white' : ''}`}>
      <h2 className={`text-2xl font-bold font-montserrat mb-4 text-[${COLORS.accent}]`}>Minting Trust in Workplaces</h2>
      <p className="text-lg leading-relaxed text-gray-600 dark:text-gray-300">
        GeoMint leverages GPS, AI, and automation to ensure every attendance record is authentic, every work hour is fair, and every payout is accurate. We solve the costly problem of **Buddy Punching** and **Time Fraud** for companies managing large field teams.
      </p>

      <div className="mt-8 grid md:grid-cols-3 gap-6">
        <div className={`p-4 border-l-4 border-[${COLORS.success}] ${isDarkMode ? 'bg-gray-700' : 'bg-gray-50'} rounded-lg`}>
          <h3 className="font-bold text-lg">AI Verification</h3>
          <p className="text-sm text-gray-600 dark:text-gray-400">Stops fraud before it starts with liveness detection and photo verification.</p>
        </div>
        <div className={`p-4 border-l-4 border-[${COLORS.secondary}] ${isDarkMode ? 'bg-gray-700' : 'bg-gray-50'} rounded-lg`}>
          <h3 className="font-bold text-lg">Adaptive Geofencing</h3>
          <p className="text-sm text-gray-600 dark:text-gray-400">Smart zones reduce false alerts and support mobile/remote work flexibility.</p>
        </div>
        <div className={`p-4 border-l-4 border-[${COLORS.accent}] ${isDarkMode ? 'bg-gray-700' : 'bg-gray-50'} rounded-lg`}>
          <h3 className="font-bold text-lg">Predictive Payroll</h3>
          <p className="text-sm text-gray-600 dark:text-gray-400">Automated payroll calculation and anomaly flagging for HR efficiency.</p>
        </div>
      </div>
    </Card>

    <Card className={`p-6 ${isDarkMode ? 'bg-[#2D3748] text-white' : ''}`}>
      <h2 className={`text-2xl font-bold font-montserrat mb-4 text-[${COLORS.primary}]`}>Contact & Scale</h2>
      <p className="text-gray-600 dark:text-gray-300">
        GeoMint is designed for enterprise scaling. Contact our team to schedule a full backend integration demo.
      </p>
      <div className="mt-4 space-y-2">
        <p className="font-semibold">Email: <span className={`text-[${COLORS.accent}]`}>hello@geomint.io</span></p>
        <p className="font-semibold">HQ: San Francisco, CA</p>
      </div>
    </Card>
  </div>
);

// Placeholder pages for simplicity
const PlaceholderPage = ({ title, isDarkMode }) => (
  <div className="space-y-8">
    <h1 className={`text-3xl font-extrabold font-montserrat ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>{title}</h1>
    <Card className={`p-10 text-center ${isDarkMode ? 'bg-[#2D3748] text-white' : ''}`}>
      <Clipboard className={`w-12 h-12 mx-auto mb-4 text-[${COLORS.accent}]`} />
      <h2 className={`text-xl font-bold text-[${COLORS.primary}] dark:text-white`}>This is the {title} Management Page</h2>
      <p className="text-gray-600 dark:text-gray-300 mt-2">
        Here, HR managers would manage employee profiles, review raw attendance data, or finalize payroll batches.
      </p>
    </Card>
  </div>
);


// --- Main App Component ---

const App = () => {
  const [view, setView] = useState('landing'); // landing, employee, admin
  const [page, setPage] = useState('dashboard'); // for admin view
  const [showModal, setShowModal] = useState(false);
  const [modalContent, setModalContent] = useState(null);
  const [isInnovationMode, setIsInnovationMode] = useState(true); // Default to ON for judges
  const [isDarkMode, setIsDarkMode] = useState(false);

  const getPageContent = () => {
    if (view === 'landing') return <LandingPage setView={setView} />;
    if (view === 'employee') return <EmployeeViewModal setView={setView} />;

    // Admin View
    const pageMap = {
      dashboard: <DashboardContent setModalContent={setModalContent} setShowModal={setShowModal} isInnovationMode={isInnovationMode} isDarkMode={isDarkMode} />,
      reports: <ReportsPage isDarkMode={isDarkMode} />,
      about: <AboutPage isDarkMode={isDarkMode} />,
      employees: <PlaceholderPage title="Employee Profiles" isDarkMode={isDarkMode} />,
      attendance: <PlaceholderPage title="Raw Attendance Data" isDarkMode={isDarkMode} />,
      payroll: <PlaceholderPage title="Payroll Batch Processing" isDarkMode={isDarkMode} />,
      geofencing: <PlaceholderPage title="Adaptive Geofencing Console" isDarkMode={isDarkMode} />,
      'predictive-payroll': <PlaceholderPage title="Predictive Payroll Forecasts" isDarkMode={isDarkMode} />,
      rewards: <PlaceholderPage title="Gamified Rewards Management" isDarkMode={isDarkMode} />,
    };
    return pageMap[page] || <DashboardContent setModalContent={setModalContent} setShowModal={setShowModal} isInnovationMode={isInnovationMode} isDarkMode={isDarkMode} />;
  };

  if (view === 'landing' || view === 'employee') {
    return getPageContent();
  }

  // Admin Layout
  return (
    <div className={`flex min-h-screen ${isDarkMode ? 'bg-[#121212] text-white' : `bg-[${COLORS.background}] text-[${COLORS.text}]`}`}>
      <Sidebar page={page} setPage={setPage} isDarkMode={isDarkMode} setIsDarkMode={setIsDarkMode} />
      <div className="flex-1 ml-64 p-8 transition-colors duration-500">
        {/* Top Navbar */}
        <div className={`flex justify-between items-center pb-6 border-b mb-8 sticky top-0 z-10 ${isDarkMode ? 'bg-[#121212] border-gray-700' : `bg-[${COLORS.background}] border-gray-200`}`}>
          <h1 className={`text-2xl font-extrabold font-montserrat ${isDarkMode ? 'text-white' : `text-[${COLORS.primary}]`}`}>{page.toUpperCase()}</h1>
          <div className="flex items-center space-x-6">
            <div className='flex items-center space-x-2'>
              <span className={`text-sm font-semibold ${isInnovationMode ? `text-[${COLORS.accent}]` : 'text-gray-500'}`}>Innovation Mode</span>
              <label className="relative inline-flex items-center cursor-pointer">
                <input
                  type="checkbox"
                  checked={isInnovationMode}
                  onChange={() => setIsInnovationMode(!isInnovationMode)}
                  className="sr-only peer"
                />
                <div className={`w-11 h-6 peer-focus:outline-none rounded-full peer ${isInnovationMode ? `bg-[${COLORS.accent}] after:translate-x-full` : 'bg-gray-400'} peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border after:rounded-full after:h-5 after:w-5 after:transition-all`}></div>
              </label>
            </div>
            <Bell className={`w-6 h-6 cursor-pointer ${isDarkMode ? 'text-white' : 'text-gray-500'} hover:text-[${COLORS.accent}]`} />
            <User className={`w-6 h-6 cursor-pointer ${isDarkMode ? 'text-white' : 'text-gray-500'} hover:text-[${COLORS.accent}]`} onClick={() => setView('landing')} />
          </div>
        </div>

        {/* Page Content */}
        {getPageContent()}
      </div>

      <Modal show={showModal} onClose={() => setShowModal(false)} title="System Alert">
        {modalContent}
      </Modal>
    </div>
  );
};

export default App;
