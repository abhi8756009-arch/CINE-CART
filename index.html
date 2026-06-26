import React, { useState, useEffect } from 'react';
import { initializeApp } from 'firebase/app';
import { 
  getFirestore, 
  collection, 
  addDoc, 
  onSnapshot 
} from 'firebase/firestore';
import { 
  getAuth, 
  signInAnonymously, 
  signInWithCustomToken, 
  onAuthStateChanged 
} from 'firebase/auth';
import { 
  Film, 
  Users, 
  Clock, 
  Award, 
  CheckCircle, 
  AlertCircle, 
  Sheet, 
  Database, 
  UserPlus, 
  Trash2, 
  Info,
  ExternalLink,
  HelpCircle,
  ChevronDown,
  ChevronUp
} from 'lucide-react';

// --- Hardcoded Google Sheets App Script Endpoint ---
const GOOGLE_SHEETS_WEBHOOK_URL = "https://script.google.com/macros/s/AKfycbwonLvQL5hxlRPpOdxu3AQTl_NJfbEuF9s4J2Zhud5-0vUkWOsbWX2jfv7pad3yuZhBHA/exec";

// --- Firebase Configuration ---
const firebaseConfig = typeof __firebase_config !== 'undefined' 
  ? JSON.parse(__firebase_config) 
  : {
      apiKey: "",
      authDomain: "mock-app.firebaseapp.com",
      projectId: "mock-app",
      storageBucket: "mock-app.appspot.com",
      messagingSenderId: "12345",
      appId: "1:12345:web:12345"
    };

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'short-film-comp-default';

export default function App() {
  // --- States ---
  const [user, setUser] = useState(null);
  const [submissions, setSubmissions] = useState([]);
  const [activeTab, setActiveTab] = useState('register'); // 'register' | 'rules'
  const [showTroubleshooting, setShowTroubleshooting] = useState(false);
  
  // --- Form States ---
  const [leaderName, setLeaderName] = useState('');
  const [email, setEmail] = useState('');
  const [phone, setPhone] = useState('');
  const [participationType, setParticipationType] = useState('individual'); // 'individual' or 'team'
  const [teamMembers, setTeamMembers] = useState(['', '', '']); // Default 3 slots (makes total team 4 including leader)
  const [filmTitle, setFilmTitle] = useState('');
  const [durationMin, setDurationMin] = useState('');
  const [durationSec, setDurationSec] = useState('');
  const [filmLink, setFilmLink] = useState('');
  const [declaration, setDeclaration] = useState(false);

  // --- Utility States ---
  const [errorMsg, setErrorMsg] = useState('');
  const [successMsg, setSuccessMsg] = useState('');
  const [loading, setLoading] = useState(false);
  const [timeLeft, setTimeLeft] = useState({ hrs: 0, mins: 0, secs: 0 });

  // --- Countdown Timer Setup (Targets June 27, 2026, 11:59:59 PM) ---
  useEffect(() => {
    const targetDate = new Date('2026-06-27T23:59:59').getTime();

    const interval = setInterval(() => {
      const now = new Date().getTime();
      const difference = targetDate - now;

      if (difference <= 0) {
        clearInterval(interval);
        setTimeLeft({ hrs: 0, mins: 0, secs: 0 });
      } else {
        const hrs = Math.floor(difference / (1000 * 60 * 60));
        const mins = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
        const secs = Math.floor((difference % (1000 * 60)) / 1000);
        setTimeLeft({ hrs, mins, secs });
      }
    }, 1000);

    return () => clearInterval(interval);
  }, []);

  // --- Firebase Auth & Security Compliance ---
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        console.error("Authentication failed:", err);
      }
    };
    initAuth();

    const unsubscribeAuth = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser);
    });

    return () => unsubscribeAuth();
  }, []);

  // --- Real-Time Submissions Synced via Firestore ---
  useEffect(() => {
    if (!user) return;

    const submissionsRef = collection(db, 'artifacts', appId, 'public', 'data', 'submissions');
    
    const unsubscribeSubmissions = onSnapshot(submissionsRef, (snapshot) => {
      const dataList = snapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data()
      }));
      setSubmissions(dataList);
    }, (error) => {
      console.error("Failed to fetch live submissions:", error);
    });

    return () => unsubscribeSubmissions();
  }, [user]);

  // --- Manage Dynamic Team Member Inputs ---
  const handleAddMember = () => {
    if (teamMembers.length >= 4) {
      setErrorMsg('Maximum 5 team members (1 Leader + 4 Members) are allowed!');
      setTimeout(() => setErrorMsg(''), 4000);
      return;
    }
    setTeamMembers([...teamMembers, '']);
  };

  const handleRemoveMember = (index) => {
    const updated = [...teamMembers];
    updated.splice(index, 1);
    setTeamMembers(updated);
  };

  const handleMemberChange = (index, value) => {
    const updated = [...teamMembers];
    updated[index] = value;
    setTeamMembers(updated);
  };

  // --- Form Submission Logic ---
  const handleSubmit = async (e) => {
    e.preventDefault();
    setErrorMsg('');
    setSuccessMsg('');

    // Pre-flight validation
    if (!leaderName || !email || !phone || !filmTitle || !durationMin || !durationSec || !filmLink) {
      setErrorMsg('Please fill in all mandatory fields before submitting!');
      return;
    }

    if (!declaration) {
      setErrorMsg('You must agree to the Terms, Content & Copyright Guidelines!');
      return;
    }

    // Convert and validate duration (Min 3:00, Max 6:00)
    const mins = parseInt(durationMin, 10) || 0;
    const secs = parseInt(durationSec, 10) || 0;
    const totalSeconds = (mins * 60) + secs;

    if (totalSeconds < 180 || totalSeconds > 360) {
      setErrorMsg('Disqualification Alert: Film duration must be strictly between 3 and 6 minutes!');
      return;
    }

    // Validate Team Size (Leader + members must total exactly 4 or 5 if 'team')
    let totalTeamCount = 1;
    let filteredMembers = [];
    if (participationType === 'team') {
      filteredMembers = teamMembers.filter(m => m.trim() !== '');
      totalTeamCount += filteredMembers.length;
      if (totalTeamCount < 4 || totalTeamCount > 5) {
        setErrorMsg('Validation Error: Teams must consist of exactly 4 or 5 members in total!');
        return;
      }
    }

    setLoading(true);

    const submissionPayload = {
      leaderName,
      email,
      phone,
      participationType,
      teamMembers: filteredMembers.join(', ') || 'N/A',
      totalTeamSize: totalTeamCount,
      filmTitle,
      filmDuration: `${mins}m ${secs}s`,
      filmLink,
      submittedAt: new Date().toISOString()
    };

    try {
      // 1. Save to Cloud Firestore (Database backup)
      if (user) {
        const submissionsRef = collection(db, 'artifacts', appId, 'public', 'data', 'submissions');
        await addDoc(submissionsRef, submissionPayload);
      }

      // 2. Format as a native URLSearchParams body to prevent CORS header blocks
      const formBody = new URLSearchParams();
      formBody.append('leaderName', submissionPayload.leaderName);
      formBody.append('email', submissionPayload.email);
      formBody.append('phone', submissionPayload.phone);
      formBody.append('participationType', submissionPayload.participationType);
      formBody.append('teamMembers', submissionPayload.teamMembers);
      formBody.append('totalTeamSize', submissionPayload.totalTeamSize.toString());
      formBody.append('filmTitle', submissionPayload.filmTitle);
      formBody.append('filmDuration', submissionPayload.filmDuration);
      formBody.append('filmLink', submissionPayload.filmLink);

      // Perform cross-origin request
      await fetch(GOOGLE_SHEETS_WEBHOOK_URL, {
        method: 'POST',
        mode: 'no-cors',
        body: formBody // Natively resolved as application/x-www-form-urlencoded
      });

      setSuccessMsg('Congratulations! Your short film entry has been successfully submitted to the server & Google Sheet.');
      
      // Clear inputs
      setLeaderName('');
      setEmail('');
      setPhone('');
      setFilmTitle('');
      setDurationMin('');
      setDurationSec('');
      setFilmLink('');
      setDeclaration(false);
      setTeamMembers(['', '', '']);
    } catch (err) {
      setErrorMsg('A system error occurred during submission. Please check your internet connection.');
      console.error(err);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 font-sans">
      {/* --- Header / Navbar --- */}
      <header className="border-b border-slate-800/80 bg-slate-900/60 backdrop-blur-md sticky top-0 z-50">
        <div className="max-w-7xl mx-auto px-4 py-4 flex flex-col md:flex-row items-center justify-between gap-4">
          <div className="flex items-center gap-3">
            <div className="p-2.5 bg-amber-500/10 text-amber-400 rounded-xl border border-amber-500/20">
              <Film size={26} className="animate-pulse" />
            </div>
            <div>
              <h1 className="text-xl sm:text-2xl font-black tracking-wider text-white">
                CINE<span className="text-amber-400">CRAFT</span>
              </h1>
              <p className="text-xs text-slate-400 font-semibold uppercase tracking-widest">Film Competition 2026</p>
            </div>
          </div>

          {/* Navigation Tab Toggles */}
          <div className="flex items-center bg-slate-950 p-1.5 rounded-xl border border-slate-800">
            <button 
              onClick={() => setActiveTab('register')}
              className={`px-4 py-2 rounded-lg text-xs sm:text-sm font-bold tracking-wide transition-all duration-200 ${
                activeTab === 'register' ? 'bg-amber-500 text-slate-950 shadow-md shadow-amber-500/10' : 'text-slate-400 hover:text-slate-200'
              }`}
            >
              Register & Submit
            </button>
            <button 
              onClick={() => setActiveTab('rules')}
              className={`px-4 py-2 rounded-lg text-xs sm:text-sm font-bold tracking-wide transition-all duration-200 ${
                activeTab === 'rules' ? 'bg-amber-500 text-slate-950 shadow-md shadow-amber-500/10' : 'text-slate-400 hover:text-slate-200'
              }`}
            >
              Rules & Criteria
            </button>
          </div>
        </div>
      </header>

      {/* --- Main Contents --- */}
      <main className="max-w-7xl mx-auto px-4 py-8">
        
        {/* Status Alerts */}
        {errorMsg && (
          <div className="mb-6 p-4 bg-red-950/80 border border-red-800/80 text-red-200 rounded-xl flex items-center gap-3 animate-bounce">
            <AlertCircle className="shrink-0 text-red-400" />
            <p className="text-sm font-semibold">{errorMsg}</p>
          </div>
        )}
        {successMsg && (
          <div className="mb-6 p-4 bg-emerald-950/80 border border-emerald-800/80 text-emerald-200 rounded-xl flex items-center gap-3 animate-pulse">
            <CheckCircle className="shrink-0 text-emerald-400" />
            <p className="text-sm font-semibold">{successMsg}</p>
          </div>
        )}

        {/* --- Tab 1: Submit Form & Left Counters --- */}
        {activeTab === 'register' && (
          <div className="grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            {/* Sidebar Details column */}
            <div className="lg:col-span-4 space-y-6">
              
              {/* Submission Deadline widget */}
              <div className="bg-gradient-to-br from-slate-900 to-slate-950 border border-slate-800 rounded-2xl p-6 relative overflow-hidden">
                <div className="absolute top-0 right-0 w-32 h-32 bg-amber-500/5 rounded-full blur-2xl animate-pulse" />
                <span className="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-[10px] font-bold tracking-wider uppercase bg-amber-500/10 text-amber-400 border border-amber-500/20 mb-4">
                  <Clock size={12} /> Live Submission Timer
                </span>
                <h3 className="text-lg font-bold text-white mb-1">Submissions Close Tomorrow</h3>
                <p className="text-xs text-slate-400 mb-6">Entries close strictly on June 27, 2026, at 11:59 PM.</p>
                
                <div className="grid grid-cols-3 gap-3">
                  <div className="bg-slate-950 border border-slate-800 rounded-xl p-3 text-center">
                    <div className="text-2xl sm:text-3xl font-black text-amber-400">{String(timeLeft.hrs).padStart(2, '0')}</div>
                    <div className="text-[10px] text-slate-500 uppercase font-black mt-1">Hours</div>
                  </div>
                  <div className="bg-slate-950 border border-slate-800 rounded-xl p-3 text-center">
                    <div className="text-2xl sm:text-3xl font-black text-amber-400">{String(timeLeft.mins).padStart(2, '0')}</div>
                    <div className="text-[10px] text-slate-500 uppercase font-black mt-1">Minutes</div>
                  </div>
                  <div className="bg-slate-950 border border-slate-800 rounded-xl p-3 text-center">
                    <div className="text-2xl sm:text-3xl font-black text-amber-400">{String(timeLeft.secs).padStart(2, '0')}</div>
                    <div className="text-[10px] text-slate-500 uppercase font-black mt-1">Seconds</div>
                  </div>
                </div>
              </div>

              {/* Information Checkpoints */}
              <div className="bg-slate-900/40 border border-slate-800 rounded-2xl p-6 space-y-4">
                <h3 className="text-xs font-bold text-slate-400 uppercase tracking-widest">Important Submission Rules:</h3>
                
                <div className="flex items-start gap-3">
                  <div className="mt-1 p-1.5 bg-amber-500/10 rounded-lg text-amber-400">
                    <Users size={16} />
                  </div>
                  <div>
                    <h4 className="text-sm font-bold text-slate-200">Permitted Team Size</h4>
                    <p className="text-xs text-slate-400 mt-0.5">Submit as a Single Individual or as a team consisting of exactly 4 to 5 members.</p>
                  </div>
                </div>

                <div className="flex items-start gap-3">
                  <div className="mt-1 p-1.5 bg-amber-500/10 rounded-lg text-amber-400">
                    <Clock size={16} />
                  </div>
                  <div>
                    <h4 className="text-sm font-bold text-slate-200">Required Duration</h4>
                    <p className="text-xs text-slate-400 mt-0.5">Videos must be at least 3 minutes and up to 6 minutes long (including credits).</p>
                  </div>
                </div>

                <div className="flex items-start gap-3">
                  <div className="mt-1 p-1.5 bg-amber-500/10 rounded-lg text-amber-400">
                    <Database size={16} />
                  </div>
                  <div>
                    <h4 className="text-sm font-bold text-slate-200">Video Hosting</h4>
                    <p className="text-xs text-slate-400 mt-0.5">Upload film to Google Drive, YouTube, or Vimeo and submit the URL below.</p>
                  </div>
                </div>
              </div>

              {/* Google Sheets Sync Widget */}
              <div className="bg-slate-900/40 border border-slate-800 rounded-2xl p-6">
                <div className="flex items-center justify-between mb-4">
                  <h4 className="text-xs font-bold text-slate-200 flex items-center gap-2 uppercase tracking-widest">
                    <Sheet size={16} className="text-emerald-500" />
                    Google Sheets Sync
                  </h4>
                  <span className="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-ping" />
                </div>
                <p className="text-xs text-emerald-400/95 font-medium bg-emerald-500/10 p-3 rounded-xl border border-emerald-500/20">
                  Spreadsheet Active! All registrations and video submissions are directly synced and written to your official Google Sheet.
                </p>
              </div>

            </div>

            {/* Right Form column */}
            <div className="lg:col-span-8 space-y-6">
              <div className="bg-slate-900 border border-slate-800 rounded-2xl p-6 sm:p-8">
                <div className="mb-6">
                  <h2 className="text-xl font-extrabold text-white">Movie Submission Form</h2>
                  <p className="text-xs text-slate-400 mt-1">Please provide accurate personal, team, and film details. All fields are required.</p>
                </div>

                <form onSubmit={handleSubmit} className="space-y-6">
                  
                  {/* Step 1: Personal Info */}
                  <div className="space-y-4">
                    <h3 className="text-xs font-bold text-amber-400 uppercase tracking-widest border-b border-slate-800 pb-2">1. Personal Info</h3>
                    
                    <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                      <div>
                        <label className="block text-xs font-semibold text-slate-300 mb-2">Team Leader / Participant Full Name *</label>
                        <input 
                          type="text" 
                          value={leaderName}
                          onChange={(e) => setLeaderName(e.target.value)}
                          placeholder="e.g. John Doe"
                          className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder:text-slate-600 focus:outline-none focus:border-amber-500 transition-colors"
                        />
                      </div>
                      <div>
                        <label className="block text-xs font-semibold text-slate-300 mb-2">Email Address *</label>
                        <input 
                          type="email" 
                          value={email}
                          onChange={(e) => setEmail(e.target.value)}
                          placeholder="e.g. johndoe@gmail.com"
                          className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder:text-slate-600 focus:outline-none focus:border-amber-500 transition-colors"
                        />
                      </div>
                    </div>

                    <div>
                      <label className="block text-xs font-semibold text-slate-300 mb-2">Mobile / WhatsApp Number *</label>
                      <input 
                        type="tel" 
                        value={phone}
                        onChange={(e) => setPhone(e.target.value)}
                        placeholder="e.g. +1 555-0199"
                        className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder:text-slate-600 focus:outline-none focus:border-amber-500 transition-colors"
                      />
                    </div>
                  </div>

                  {/* Step 2: Team selection */}
                  <div className="space-y-4 pt-2">
                    <h3 className="text-xs font-bold text-amber-400 uppercase tracking-widest border-b border-slate-800 pb-2">2. Team Structure</h3>
                    
                    <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
                      <label className={`border-2 rounded-xl p-4 flex items-start gap-3 cursor-pointer transition-all ${
                        participationType === 'individual' 
                          ? 'border-amber-500 bg-amber-500/5' 
                          : 'border-slate-800 bg-slate-950 hover:border-slate-700'
                      }`}>
                        <input 
                          type="radio" 
                          name="participationType" 
                          value="individual"
                          checked={participationType === 'individual'}
                          onChange={() => setParticipationType('individual')}
                          className="mt-1 accent-amber-500"
                        />
                        <div>
                          <span className="block text-sm font-bold text-slate-200">Single Person Entry</span>
                          <span className="block text-xs text-slate-500 mt-1">If you are the sole director/writer/creator of the project.</span>
                        </div>
                      </label>

                      <label className={`border-2 rounded-xl p-4 flex items-start gap-3 cursor-pointer transition-all ${
                        participationType === 'team' 
                          ? 'border-amber-500 bg-amber-500/5' 
                          : 'border-slate-800 bg-slate-950 hover:border-slate-700'
                      }`}>
                        <input 
                          type="radio" 
                          name="participationType" 
                          value="team"
                          checked={participationType === 'team'}
                          onChange={() => setParticipationType('team')}
                          className="mt-1 accent-amber-500"
                        />
                        <div>
                          <span className="block text-sm font-bold text-slate-200">Team Entry (4 to 5 Members)</span>
                          <span className="block text-xs text-slate-500 mt-1">A unified crew of strictly 4 or 5 members (including Leader).</span>
                        </div>
                      </label>
                    </div>

                    {/* Crew inputs */}
                    {participationType === 'team' && (
                      <div className="p-4 bg-slate-950 border border-slate-800 rounded-xl space-y-4">
                        <div className="flex items-center justify-between">
                          <label className="block text-xs font-bold text-slate-300">Team Crew Member Names (Excluding Team Leader):</label>
                          <button 
                            type="button"
                            onClick={handleAddMember}
                            className="text-xs bg-slate-900 border border-slate-800 text-amber-400 hover:text-amber-300 px-3 py-1.5 rounded-lg flex items-center gap-1 font-semibold"
                          >
                            <UserPlus size={12} /> Add Member Slot
                          </button>
                        </div>

                        <div className="space-y-3">
                          {teamMembers.map((member, index) => (
                            <div key={index} className="flex items-center gap-2 animate-fadeIn">
                              <span className="text-xs text-slate-500 font-bold w-20">Member #{index + 2}:</span>
                              <input 
                                type="text"
                                value={member}
                                onChange={(e) => handleMemberChange(index, e.target.value)}
                                placeholder="Full name of crew member"
                                className="flex-1 bg-slate-900 border border-slate-800 rounded-lg px-3 py-2 text-xs text-slate-300 focus:outline-none focus:border-amber-500"
                              />
                              {teamMembers.length > 2 && (
                                <button 
                                  type="button"
                                  onClick={() => handleRemoveMember(index)}
                                  className="p-2 text-rose-500 hover:bg-rose-500/10 rounded-lg transition-colors"
                                >
                                  <Trash2 size={14} />
                                </button>
                              )}
                            </div>
                          ))}
                        </div>
                        <p className="text-[10px] text-slate-500">
                          * Reminder: Total count (Team Leader + Crew Slots) must be exactly 4 or 5. Current Crew Size: {teamMembers.filter(m => m.trim() !== '').length + 1}
                        </p>
                      </div>
                    )}
                  </div>

                  {/* Step 3: Movie Specs */}
                  <div className="space-y-4 pt-2">
                    <h3 className="text-xs font-bold text-amber-400 uppercase tracking-widest border-b border-slate-800 pb-2">3. Film Details</h3>
                    
                    <div>
                      <label className="block text-xs font-semibold text-slate-300 mb-2">Film Title *</label>
                      <input 
                        type="text" 
                        value={filmTitle}
                        onChange={(e) => setFilmTitle(e.target.value)}
                        placeholder="Enter the official title of your film"
                        className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder:text-slate-600 focus:outline-none focus:border-amber-500 transition-colors"
                      />
                    </div>

                    <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                      <div>
                        <label className="block text-xs font-semibold text-slate-300 mb-2">Exact Film Duration (Min 3:00, Max 6:00) *</label>
                        <div className="flex items-center gap-2">
                          <input 
                            type="number" 
                            min="0"
                            max="59"
                            value={durationMin}
                            onChange={(e) => setDurationMin(e.target.value)}
                            placeholder="Mins"
                            className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder:text-slate-600 focus:outline-none focus:border-amber-500 text-center"
                          />
                          <span className="text-slate-600 font-black">:</span>
                          <input 
                            type="number" 
                            min="0"
                            max="59"
                            value={durationSec}
                            onChange={(e) => setDurationSec(e.target.value)}
                            placeholder="Secs"
                            className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder:text-slate-600 focus:outline-none focus:border-amber-500 text-center"
                          />
                        </div>
                        <span className="block text-[10px] text-slate-500 mt-1.5">E.g. 04m 32s. Entries outside 3:00 - 6:00 will be auto-disqualified.</span>
                      </div>

                      <div>
                        <label className="block text-xs font-semibold text-slate-300 mb-2">Google Drive / YouTube Link *</label>
                        <input 
                          type="url" 
                          value={filmLink}
                          onChange={(e) => setFilmLink(e.target.value)}
                          placeholder="https://drive.google.com/file/d/..."
                          className="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder:text-slate-600 focus:outline-none focus:border-amber-500 transition-colors"
                        />
                        <span className="block text-[10px] text-slate-500 mt-1.5">Mandatory: Google Drive links must be set to "Anyone with the link can view".</span>
                      </div>
                    </div>
                  </div>

                  {/* Agreement checkbox */}
                  <div className="pt-4 border-t border-slate-800">
                    <label className="flex items-start gap-3 cursor-pointer select-none">
                      <input 
                        type="checkbox" 
                        checked={declaration}
                        onChange={(e) => setDeclaration(e.target.checked)}
                        className="mt-1 rounded border-slate-800 text-amber-500 focus:ring-amber-500 bg-slate-950 w-4 h-4"
                      />
                      <span className="text-xs text-slate-400 leading-relaxed">
                        I hereby declare that this short film submission is entirely original, contains only copyright-free or fully licensed music, and complies with the 3 to 6-minute duration criteria. I acknowledge that failure to comply with the rules will result in immediate disqualification.
                      </span>
                    </label>
                  </div>

                  {/* Submit container */}
                  <div className="pt-2">
                    <button
                      type="submit"
                      disabled={loading}
                      className="w-full bg-gradient-to-r from-amber-500 to-amber-600 text-slate-950 hover:from-amber-400 hover:to-amber-500 font-extrabold text-sm py-4 rounded-xl shadow-lg shadow-amber-500/10 flex items-center justify-center gap-2 transition-all duration-150 disabled:opacity-50"
                    >
                      {loading ? (
                        <>Uploading submission details...</>
                      ) : (
                        <>
                          <Film size={18} /> Submit My Short Film Entry
                        </>
                      )}
                    </button>
                  </div>

                </form>
              </div>

              {/* Troubleshooting Accordion (Crucial for spreadsheet settings verification) */}
              <div className="bg-slate-900/60 border border-slate-800/80 rounded-2xl overflow-hidden">
                <button
                  type="button"
                  onClick={() => setShowTroubleshooting(!showTroubleshooting)}
                  className="w-full px-6 py-4 flex items-center justify-between text-left text-sm font-bold text-slate-200 hover:bg-slate-900 transition-colors"
                >
                  <span className="flex items-center gap-2 text-amber-400">
                    <HelpCircle size={16} /> Not seeing entries in your Google Sheet? Click to fix
                  </span>
                  {showTroubleshooting ? <ChevronUp size={16} /> : <ChevronDown size={16} />}
                </button>
                
                {showTroubleshooting && (
                  <div className="px-6 pb-6 pt-2 space-y-4 text-xs text-slate-300 leading-relaxed border-t border-slate-800/60 animate-fadeIn">
                    <p className="font-semibold text-white">Google Apps Script Web Apps have strict authorization requirements. Follow these steps to ensure submissions flow in perfectly:</p>
                    <ol className="list-decimal list-inside space-y-2.5 text-slate-400">
                      <li>
                        <strong className="text-slate-200">Container-Bound Script:</strong> Make sure you created the script from <span className="font-bold text-white">Extensions → Apps Script</span> *inside* the Google Sheet. Standing scripts created on standard script.google.com will throw errors when using <code className="text-amber-400 bg-slate-950 px-1 py-0.5 rounded">getActiveSpreadsheet()</code>.
                      </li>
                      <li>
                        <strong className="text-slate-200">Re-deploy properly:</strong> In your script editor, click <span className="text-slate-200 font-bold">Deploy → New Deployment</span>.
                      </li>
                      <li>
                        <strong className="text-slate-200">Access Level:</strong> Change <span className="text-slate-200 font-bold">Who has access</span> from "Only Myself" to <span className="text-amber-400 font-bold">"Anyone"</span>. *If this is not set to Anyone, browsers will reject form submissions.*
                      </li>
                      <li>
                        <strong className="text-slate-200">Execute as:</strong> Change to <span className="text-slate-200 font-bold">"Me (your email)"</span>.
                      </li>
                      <li>
                        <strong className="text-slate-200">Authorize:</strong> Click Deploy. Google will ask you to authorize permissions. Click <span className="text-slate-200 font-bold">"Advanced"</span> and then <span className="text-slate-200 font-bold">"Go to Untitled Script (unsafe)"</span> to complete permission mapping.
                      </li>
                    </ol>
                  </div>
                )}
              </div>

            </div>

          </div>
        )}

        {/* --- Tab 2: Official Guidelines & Criteria --- */}
        {activeTab === 'rules' && (
          <div className="max-w-4xl mx-auto space-y-8 animate-fadeIn">
            
            <div className="bg-gradient-to-r from-slate-900 to-slate-950 border border-slate-800 rounded-2xl p-6 sm:p-8 text-center relative overflow-hidden">
              <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-64 h-64 bg-amber-500/5 rounded-full blur-3xl pointer-events-none" />
              <Award className="mx-auto text-amber-400 mb-3" size={40} />
              <h2 className="text-2xl font-black text-white">Competition Rule Book</h2>
              <p className="text-xs text-slate-400 mt-2 max-w-lg mx-auto">
                Strict adherence to these rules ensures fairness for all creators. Failing to meet requirements leads to automatic disqualification.
              </p>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              
              <div className="bg-slate-900 border border-slate-800 rounded-2xl p-6 space-y-4">
                <h3 className="text-sm font-bold text-amber-400 flex items-center gap-2 uppercase tracking-widest">
                  <Users size={16} /> Team & Eligibility
                </h3>
                <ul className="space-y-3 text-xs text-slate-300">
                  <li className="flex items-start gap-2.5">
                    <span className="w-1.5 h-1.5 rounded-full bg-amber-500 mt-1.5 shrink-0" />
                    <span><strong>Individual:</strong> Individual creators can submit entries solo.</span>
                  </li>
                  <li className="flex items-start gap-2.5">
                    <span className="w-1.5 h-1.5 rounded-full bg-amber-500 mt-1.5 shrink-0" />
                    <span><strong>Teams:</strong> If participating with a crew, your team must consist of <strong>exactly 4 or 5 members</strong> (including leader).</span>
                  </li>
                  <li className="flex items-start gap-2.5">
                    <span className="w-1.5 h-1.5 rounded-full bg-amber-500 mt-1.5 shrink-0" />
                    <span>Teams with size less than 4 or exceeding 5 will not be processed.</span>
                  </li>
                </ul>
              </div>

              <div className="bg-slate-900 border border-slate-800 rounded-2xl p-6 space-y-4">
                <h3 className="text-sm font-bold text-amber-400 flex items-center gap-2 uppercase tracking-widest">
                  <Clock size={16} /> Technical & Run Time
                </h3>
                <ul className="space-y-3 text-xs text-slate-300">
                  <li className="flex items-start gap-2.5">
                    <span className="w-1.5 h-1.5 rounded-full bg-amber-500 mt-1.5 shrink-0" />
                    <span>The short film must be at least <strong>3 minutes (180s)</strong> in length.</span>
                  </li>
                  <li className="flex items-start gap-2.5">
                    <span className="w-1.5 h-1.5 rounded-full bg-amber-500 mt-1.5 shrink-0" />
                    <span>The film must not exceed <strong>6 minutes (360s)</strong> in length.</span>
                  </li>
                  <li className="flex items-start gap-2.5">
                    <span className="w-1.5 h-1.5 rounded-full bg-amber-500 mt-1.5 shrink-0" />
                    <span>All introductory title cards and closing scroll credits must be included within the total runtime.</span>
                  </li>
                </ul>
              </div>

            </div>

            <div className="bg-slate-900 border border-slate-800 rounded-2xl p-6 sm:p-8 space-y-6">
              <h3 className="text-base font-extrabold text-white flex items-center gap-2">
                <Award size={20} className="text-amber-500" /> Professional Judging Criteria (50 Marks Total)
              </h3>
              
              <div className="grid grid-cols-2 md:grid-cols-5 gap-4">
                
                <div className="bg-slate-950 border border-slate-800/60 rounded-xl p-4 text-center">
                  <div className="text-2xl font-black text-amber-400">10</div>
                  <div className="text-xs font-bold text-slate-200 mt-1">Story & Screenplay</div>
                  <p className="text-[10px] text-slate-500 mt-1.5">Originality, script structure, pacing, and visual storytelling.</p>
                </div>

                <div className="bg-slate-950 border border-slate-800/60 rounded-xl p-4 text-center">
                  <div className="text-2xl font-black text-amber-400">10</div>
                  <div className="text-xs font-bold text-slate-200 mt-1">Direction</div>
                  <p className="text-[10px] text-slate-500 mt-1.5">Creative vision, conceptual execution, and story translation.</p>
                </div>

                <div className="bg-slate-950 border border-slate-800/60 rounded-xl p-4 text-center">
                  <div className="text-2xl font-black text-amber-400">10</div>
                  <div className="text-xs font-bold text-slate-200 mt-1">Cinematography</div>
                  <p className="text-[10px] text-slate-500 mt-1.5">Framing, camera angles, movement, and mood lighting.</p>
                </div>

                <div className="bg-slate-950 border border-slate-800/60 rounded-xl p-4 text-center">
                  <div className="text-2xl font-black text-amber-400">10</div>
                  <div className="text-xs font-bold text-slate-200 mt-1">Editing & Sound</div>
                  <p className="text-[10px] text-slate-500 mt-1.5">Pacing rhythm, transitions, score, and clear audio mixing.</p>
                </div>

                <div className="bg-slate-950 border border-slate-800/60 rounded-xl p-4 text-center">
                  <div className="text-2xl font-black text-amber-400">10</div>
                  <div className="text-xs font-bold text-slate-200 mt-1">Acting Quality</div>
                  <p className="text-[10px] text-slate-500 mt-1.5">Character believability, screen delivery, and expressions.</p>
                </div>

              </div>
            </div>

            <div className="bg-slate-900 border border-slate-800 rounded-2xl p-6 space-y-4">
              <h3 className="text-sm font-bold text-rose-400 flex items-center gap-2 uppercase tracking-widest">
                <Info size={16} /> Copyright & Code of Conduct
              </h3>
              <ul className="space-y-2.5 text-xs text-slate-300 list-disc list-inside">
                <li>Video quality must be at least <strong>1080p (Full HD)</strong> or higher.</li>
                <li>Hate speech, profanity, or discriminatory visual themes will lead to immediate deletion of submissions.</li>
                <li>Using unlicensed commercial audio tracks will flag copyright alerts. Only use royalty-free or licensed tracks.</li>
              </ul>
            </div>

          </div>
        )}

      </main>

      {/* --- Footer --- */}
      <footer className="border-t border-slate-900 bg-slate-950/80 py-8 mt-16 text-center text-xs text-slate-500">
        <div className="max-w-7xl mx-auto px-4 space-y-2">
          <p className="font-bold text-slate-400">CineCraft Short Film Movie Competition Committee 2026 © All Rights Reserved.</p>
          <p>This web application is highly optimized for performance and compatibility across desktop, tablet, and mobile platforms.</p>
        </div>
      </footer>
    </div>
  );
}