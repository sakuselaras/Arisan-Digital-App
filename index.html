import React, { useState, useEffect, createContext, useContext } from 'react';
import { 
  Home, Users, Layers, Activity, Clock, Send, 
  Copy, Plus, AlertTriangle, CheckCircle, XCircle, 
  Menu, LogOut, RefreshCw, X 
} from 'lucide-react';

// === KONFIGURASI SUPABASE ===
// Pastikan ini TRUE agar langsung hidup saat di Vercel
const NYALAKAN_SUPABASE_DI_VSCODE = true; 

let supabase = null;
const SUPABASE_URL = 'https://mynunojhzwrnxvhpslgn.supabase.co'; 
const SUPABASE_KEY = 'sb_publishable_yyx4DTIuc_mnDAULw-DGHA_300K7Y6_';

if (NYALAKAN_SUPABASE_DI_VSCODE) {
   // Memuat modul secara dinamis agar tidak menyebabkan error kompilasi
   import('@supabase/supabase-js').then((module) => {
       supabase = module.createClient(SUPABASE_URL, SUPABASE_KEY);
   }).catch(e => console.warn('Supabase JS module loading failed:', e));
}

// Pelindung LocalStorage (Mode Fallback)
const safeStorage = {
  get: (key, fallback = null) => {
    try { const item = localStorage.getItem(key); return item ? JSON.parse(item) : fallback; } 
    catch (e) { return fallback; }
  },
  set: (key, value) => {
    try { localStorage.setItem(key, JSON.stringify(value)); } catch (e) {}
  },
  remove: (key) => {
    try { localStorage.removeItem(key); } catch (e) {}
  }
};

// Utils
const generateId = () => Math.random().toString(36).substring(2, 11);
const generateTenantCode = () => Math.random().toString(36).substring(2, 8).toUpperCase();
const formatRp = (num) => new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(num || 0);
const formatDate = (dateString) => {
  if (!dateString) return '-';
  return new Date(dateString).toLocaleDateString('id-ID', { day: '2-digit', month: 'short', year: 'numeric' });
};

// API Bridge
const api = {
  delay: (ms = 400) => new Promise(res => setTimeout(res, ms)),

  get: async (table, tenantId) => {
    if (NYALAKAN_SUPABASE_DI_VSCODE && supabase) {
      const { data, error } = await supabase.from(table).select('*').eq('tenantId', tenantId);
      if (error) throw error;
      return data || [];
    }
    await api.delay();
    const allData = safeStorage.get(`saas_${table}`, []);
    return allData.filter(d => d.tenantId === tenantId || d.tenantCode === tenantId); 
  },

  insert: async (table, data) => {
    if (NYALAKAN_SUPABASE_DI_VSCODE && supabase) {
      const { data: result, error } = await supabase.from(table).insert([data]).select();
      if (error) throw error;
      return result[0];
    }
    await api.delay();
    const allData = safeStorage.get(`saas_${table}`, []);
    allData.push(data);
    safeStorage.set(`saas_${table}`, allData);
    return data;
  },

  update: async (table, id, data) => {
    if (NYALAKAN_SUPABASE_DI_VSCODE && supabase) {
      const { data: result, error } = await supabase.from(table).update(data).eq('id', id).select();
      if (error) throw error;
      return result[0];
    }
    await api.delay();
    const allData = safeStorage.get(`saas_${table}`, []);
    const index = allData.findIndex(d => d.id === id);
    if(index > -1) {
       allData[index] = { ...allData[index], ...data };
       safeStorage.set(`saas_${table}`, allData);
    }
  },

  login: async (email, password) => {
    if (NYALAKAN_SUPABASE_DI_VSCODE && supabase) {
       const { data, error } = await supabase.from('users').select('*').eq('email', email).eq('password', btoa(password)).single();
       if (error || !data) throw new Error("Email atau password salah");
       return data;
    }
    await api.delay();
    const users = safeStorage.get('saas_users', []);
    const user = users.find(u => u.email === email && u.password === btoa(password));
    if(!user) throw new Error("Email atau password salah");
    return user;
  },
  
  getTenantByCode: async (tenantCode) => {
    if(!tenantCode) throw new Error("Kode tidak valid");
    if (NYALAKAN_SUPABASE_DI_VSCODE && supabase) {
       const { data, error } = await supabase.from('tenants').select('*').eq('code', tenantCode).single();
       if (error || !data) throw new Error("Kode Arisan tidak ditemukan");
       return data;
    }
    await api.delay();
    const tenants = safeStorage.get('saas_tenants', []);
    const tenant = tenants.find(t => t.code === tenantCode);
    if(!tenant) throw new Error("Kode Arisan tidak valid atau tidak ditemukan");
    return tenant;
  }
};

const DataContext = createContext();

const DataProvider = ({ children }) => {
  const [data, setDataState] = useState({ users: [], profiles: [], groups: [], groupMembers: [], deposits: [], draws: [], payouts: [], logs: [], tenant: null });
  const [currentUser, setCurrentUser] = useState(null);
  const [toastMsg, setToastMsg] = useState(null);
  const [isLoading, setIsLoading] = useState(true);

  const showToast = (msg, type = 'success') => {
    setToastMsg({ msg, type });
    setTimeout(() => setToastMsg(null), 3000);
  };

  const loadData = async (user) => {
    setIsLoading(true);
    try {
      if(!user || !user.tenantId) {
         setIsLoading(false);
         return;
      }
      const tenantId = user.tenantId;
      const [ users, profiles, groups, groupMembers, deposits, draws, payouts, logs, tenants ] = await Promise.all([
        api.get('users', tenantId),
        api.get('profiles', tenantId),
        api.get('groups', tenantId),
        api.get('group_members', tenantId),
        api.get('deposits', tenantId),
        api.get('draws', tenantId),
        api.get('payouts', tenantId),
        api.get('logs', tenantId),
        !NYALAKAN_SUPABASE_DI_VSCODE ? safeStorage.get('saas_tenants', []) : [] 
      ]);

      const myTenant = NYALAKAN_SUPABASE_DI_VSCODE ? await api.getTenantByCode(user.tenantCode) : tenants.find(t => t.id === tenantId);

      setDataState({
        users: users || [], profiles: profiles || [], groups: groups || [], groupMembers: groupMembers || [], 
        deposits: deposits || [], draws: draws || [], payouts: payouts || [], 
        logs: (logs || []).sort((a,b)=>new Date(b.date)-new Date(a.date)), tenant: myTenant
      });
      setCurrentUser(user);
    } catch (err) {
      console.error(err);
      showToast("Gagal mengambil data sistem", "error");
    } finally {
      setIsLoading(false);
    }
  };

  useEffect(() => {
    const sessionUser = safeStorage.get('saas_current_user');
    if (sessionUser) loadData(sessionUser);
    else setIsLoading(false);
  }, []);

  const handleLoginSuccess = (user) => {
    safeStorage.set('saas_current_user', user);
    loadData(user);
  };

  const logout = () => {
    safeStorage.remove('saas_current_user');
    setCurrentUser(null);
    setDataState({users: [], profiles: [], groups: [], groupMembers: [], deposits: [], draws: [], payouts: [], logs: [], tenant: null});
  };

  const performAction = async (actionFn, logAction, logDetails) => {
     try {
        await actionFn();
        if(logAction && currentUser) {
           await api.insert('logs', { id: generateId(), tenantId: currentUser.tenantId, date: new Date().toISOString(), action: logAction, details: logDetails, userRole: currentUser.role });
        }
        if(currentUser) await loadData(currentUser);
     } catch (err) {
        showToast(err.message, "error");
     }
  }

  return (
    <DataContext.Provider value={{ data, currentUser, handleLoginSuccess, logout, performAction, showToast, isLoading }}>
      {children}
      {toastMsg && (
        <div className={`fixed bottom-4 right-4 p-4 rounded-xl shadow-xl text-white z-50 flex items-center gap-2 animate-bounce ${toastMsg.type === 'error' ? 'bg-red-500' : 'bg-gray-800'}`}>
          {toastMsg.type === 'error' ? <AlertTriangle size={20}/> : <CheckCircle size={20} className="text-emerald-400"/>}
          {toastMsg.msg}
        </div>
      )}
    </DataContext.Provider>
  );
};

const Card = ({ children, className = '' }) => <div className={`bg-white p-6 rounded-2xl shadow-sm border border-gray-100 ${className}`}>{children}</div>;
const Button = ({ children, onClick, variant = 'primary', className = '', disabled, type='button' }) => {
  const base = "px-4 py-2.5 rounded-xl font-bold transition-all flex items-center justify-center gap-2 disabled:opacity-50 active:scale-95";
  const variants = {
    primary: "bg-emerald-600 hover:bg-emerald-700 text-white shadow-md",
    secondary: "bg-gray-100 hover:bg-gray-200 text-gray-800",
    danger: "bg-red-500 hover:bg-red-600 text-white shadow-md shadow-red-200",
    outline: "border-2 border-emerald-600 text-emerald-600 hover:bg-emerald-50",
  };
  return <button type={type} onClick={onClick} disabled={disabled} className={`${base} ${variants[variant]} ${className}`}>{children}</button>;
};
const Input = ({ label, type = 'text', value, onChange, placeholder, required, disabled }) => (
  <div className="mb-4">
    {label && <label className="block text-sm font-bold text-gray-700 mb-1">{label}</label>}
    <input type={type} value={value} onChange={onChange} placeholder={placeholder} required={required} disabled={disabled} className="w-full px-4 py-3 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 disabled:bg-gray-50 outline-none transition-all" />
  </div>
);
const Select = ({ label, value, onChange, options, required, disabled }) => (
  <div className="mb-4">
    {label && <label className="block text-sm font-bold text-gray-700 mb-1">{label}</label>}
    <select value={value} onChange={onChange} required={required} disabled={disabled} className="w-full px-4 py-3 rounded-xl border border-gray-300 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 disabled:bg-gray-50 outline-none transition-all">
      <option value="">Pilih...</option>
      {options.map(opt => <option key={opt.value || opt} value={opt.value || opt}>{opt.label || opt}</option>)}
    </select>
  </div>
);
const Badge = ({ children, type }) => {
  const colors = { success: 'bg-emerald-100 text-emerald-800 border-emerald-200', warning: 'bg-amber-100 text-amber-800 border-amber-200', danger: 'bg-red-100 text-red-800 border-red-200', info: 'bg-blue-100 text-blue-800 border-blue-200', default: 'bg-gray-100 text-gray-800 border-gray-200' };
  return <span className={`px-2.5 py-1 rounded-full text-xs font-bold border ${colors[type] || colors.default}`}>{children}</span>;
};
const Modal = ({ isOpen, onClose, title, children }) => {
  if (!isOpen) return null;
  return (
    <div className="fixed inset-0 bg-black/60 z-50 flex items-center justify-center p-4 backdrop-blur-sm">
      <div className="bg-white rounded-3xl w-full max-w-lg shadow-2xl overflow-hidden flex flex-col max-h-[90vh]">
        <div className="p-5 border-b flex justify-between items-center bg-gray-50/50">
          <h3 className="text-lg font-bold text-gray-900">{title}</h3>
          <button onClick={onClose} className="text-gray-400 hover:text-gray-700 bg-white rounded-full p-1 shadow-sm"><X size={20}/></button>
        </div>
        <div className="p-6 overflow-y-auto">{children}</div>
      </div>
    </div>
  );
};

const AuthPage = () => {
  const { handleLoginSuccess, showToast } = useContext(DataContext);
  const [isLogin, setIsLogin] = useState(true);
  const [roleReg, setRoleReg] = useState('anggota');
  const [loading, setLoading] = useState(false);
  const [formData, setFormData] = useState({ email: '', password: '', name: '', tenantCode: '', tenantName: '' });

  const handleSubmit = async (e) => {
    e.preventDefault();
    setLoading(true);
    try {
      if (isLogin) {
        const user = await api.login(formData.email, formData.password);
        handleLoginSuccess(user);
        showToast(`Selamat datang, ${user.name}`);
      } else {
        let tenantId = '', assignedTenantCode = '';
        if (roleReg === 'admin') {
           tenantId = generateId();
           assignedTenantCode = generateTenantCode();
           await api.insert('tenants', { id: tenantId, code: assignedTenantCode, name: formData.tenantName, createdAt: new Date().toISOString() });
        } else {
           if(!formData.tenantCode) throw new Error("Kode Arisan harus diisi!");
           const tenant = await api.getTenantByCode(formData.tenantCode.toUpperCase());
           tenantId = tenant.id;
           assignedTenantCode = tenant.code;
        }

        const newUser = { 
           id: generateId(), tenantId, tenantCode: assignedTenantCode, 
           email: formData.email, password: btoa(formData.password), role: roleReg, name: formData.name, createdAt: new Date().toISOString() 
        };
        await api.insert('users', newUser);
        
        showToast(roleReg === 'admin' ? `Berhasil! Ruang dibuat dengan KODE: ${assignedTenantCode}` : 'Berhasil mendaftar! Silakan login.');
        setIsLogin(true);
        setFormData({email: '', password: '', name: '', tenantCode: '', tenantName: ''});
      }
    } catch (error) {
      showToast(error.message, 'error');
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen bg-gray-50 flex items-center justify-center p-4">
      <div className="w-full max-w-md bg-white rounded-[2rem] shadow-2xl overflow-hidden border border-gray-100">
        <div className="bg-emerald-950 p-8 text-center text-white relative overflow-hidden">
          <div className="absolute top-0 right-0 w-32 h-32 bg-emerald-900 rounded-bl-[100px] opacity-50"></div>
          <Layers size={48} className="mx-auto mb-4 relative z-10 text-emerald-400" />
          <h1 className="text-3xl font-black tracking-tight relative z-10">ArisanDigital</h1>
          <p className="text-emerald-300 mt-2 text-sm relative z-10 font-medium">Sistem Arisan Digital SaaS</p>
        </div>
        
        <div className="p-8">
          <h2 className="text-2xl font-bold text-gray-800 mb-6">{isLogin ? 'Masuk ke Akun' : 'Daftar Baru'}</h2>
          
          {!isLogin && (
             <div className="flex bg-gray-100 p-1 rounded-xl mb-6">
               <button type="button" onClick={()=>setRoleReg('anggota')} className={`flex-1 py-2 text-sm font-bold rounded-lg transition-all ${roleReg === 'anggota' ? 'bg-white shadow text-gray-900' : 'text-gray-500 hover:text-gray-700'}`}>Anggota</button>
               <button type="button" onClick={()=>setRoleReg('admin')} className={`flex-1 py-2 text-sm font-bold rounded-lg transition-all ${roleReg === 'admin' ? 'bg-white shadow text-gray-900' : 'text-gray-500 hover:text-gray-700'}`}>Buat Ruang (Admin)</button>
             </div>
          )}

          <form onSubmit={handleSubmit}>
            {!isLogin && <Input label="Nama Lengkap" value={formData.name} onChange={e=>setFormData({...formData, name: e.target.value})} required />}
            <Input label="Email" type="email" value={formData.email} onChange={e=>setFormData({...formData, email: e.target.value})} required />
            <Input label="Password" type="password" value={formData.password} onChange={e=>setFormData({...formData, password: e.target.value})} required />
            
            {!isLogin && roleReg === 'admin' && <Input label="Nama Ruang / Komunitas" placeholder="Misal: Arisan Keluarga Besar" value={formData.tenantName} onChange={e=>setFormData({...formData, tenantName: e.target.value})} required />}
            {!isLogin && roleReg === 'anggota' && <Input label="Kode Arisan (Dari Admin)" placeholder="Misal: A8B9CX" value={formData.tenantCode} onChange={e=>setFormData({...formData, tenantCode: e.target.value})} required />}

            <Button type="submit" disabled={loading} className="w-full py-4 text-lg mt-6 bg-emerald-600 hover:bg-emerald-700 shadow-xl shadow-emerald-600/20 border-none">
               {loading ? 'Memproses...' : isLogin ? 'Masuk' : 'Daftar Sekarang'}
            </Button>
          </form>

          <p className="text-center mt-8 text-gray-500 text-sm font-medium">
            {isLogin ? "Belum punya akun? " : "Sudah punya akun? "}
            <button onClick={() => setIsLogin(!isLogin)} className="text-emerald-950 font-bold hover:underline">{isLogin ? 'Daftar disini' : 'Login disini'}</button>
          </p>
        </div>
      </div>
    </div>
  );
};

const AdminDashboard = () => {
  const { data, showToast } = useContext(DataContext);
  
  const copyTenantCode = () => {
    const code = data.tenant?.code;
    if (!code) return;

    // Fallback jika API Clipboard modern diblokir oleh browser
    const fallbackCopy = (text) => {
      const textArea = document.createElement("textarea");
      textArea.value = text;
      textArea.style.top = "0";
      textArea.style.left = "0";
      textArea.style.position = "fixed";
      document.body.appendChild(textArea);
      textArea.focus();
      textArea.select();
      try {
        const successful = document.execCommand('copy');
        if (successful) showToast('Kode berhasil disalin!');
        else showToast('Gagal menyalin kode', 'error');
      } catch (err) {
        showToast('Gagal menyalin', 'error');
      }
      document.body.removeChild(textArea);
    };

    if (navigator.clipboard && window.isSecureContext) {
       navigator.clipboard.writeText(code)
        .then(() => showToast('Kode berhasil disalin!'))
        .catch(() => fallbackCopy(code));
    } else {
       fallbackCopy(code);
    }
  };

  return (
    <div className="space-y-6">
      <div className="flex flex-col md:flex-row justify-between md:items-end gap-4 mb-8">
         <div>
            <h2 className="text-3xl font-black text-gray-900 tracking-tight">Dashboard Admin</h2>
            <p className="text-gray-500 mt-1">Organisasi: <strong>{data.tenant?.name || 'Memuat...'}</strong></p>
         </div>
         <div className="bg-white px-5 py-4 rounded-2xl shadow-sm border border-emerald-100 flex items-center gap-4">
            <div>
               <p className="text-xs text-emerald-600 font-bold uppercase tracking-wider">KODE UNDANGAN ARISAN</p>
               <p className="text-3xl font-black text-gray-900 tracking-widest mt-1">{data.tenant?.code || '-'}</p>
            </div>
            <button onClick={copyTenantCode} className="p-4 bg-emerald-50 hover:bg-emerald-100 text-emerald-700 rounded-xl transition-colors"><Copy size={24}/></button>
         </div>
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        <Card className="flex items-center gap-4 bg-blue-50/50 border-blue-100"><div className="p-4 rounded-xl bg-blue-100 text-blue-600"><Users size={24}/></div><div><p className="text-sm text-gray-500 font-bold">Total Anggota</p><p className="text-2xl font-black text-gray-900">{data.users.filter(u=>u.role==='anggota').length}</p></div></Card>
        <Card className="flex items-center gap-4 bg-emerald-50/50 border-emerald-100"><div className="p-4 rounded-xl bg-emerald-100 text-emerald-600"><Layers size={24}/></div><div><p className="text-sm text-gray-500 font-bold">Grup Aktif</p><p className="text-2xl font-black text-gray-900">{data.groups.filter(g => g.status === 'Aktif').length}</p></div></Card>
        <Card className="flex items-center gap-4 bg-amber-50/50 border-amber-100"><div className="p-4 rounded-xl bg-amber-100 text-amber-600"><Clock size={24}/></div><div><p className="text-sm text-gray-500 font-bold">Setoran Menunggu</p><p className="text-2xl font-black text-gray-900">{data.deposits.filter(d => d.status === 'Menunggu Verifikasi').length}</p></div></Card>
        <Card className="flex items-center gap-4 bg-gray-50 border-gray-200"><div className="p-4 rounded-xl bg-gray-200 text-gray-600"><Send size={24}/></div><div><p className="text-sm text-gray-500 font-bold">Pencairan Tertunda</p><p className="text-2xl font-black text-gray-900">{data.payouts.filter(p => p.status === 'Menunggu').length}</p></div></Card>
      </div>

      <Card>
        <h3 className="font-bold text-lg mb-4 flex items-center gap-2 text-gray-800"><Activity size={20}/> Aktivitas Sistem Terbaru</h3>
        <div className="space-y-4">
          {data.logs.slice(0, 5).map(log => (
            <div key={log.id} className="flex justify-between items-start border-b border-gray-100 pb-4 text-sm last:border-0">
              <div><p className="font-bold text-gray-900">{log.action}</p><p className="text-gray-500 font-medium">{log.details}</p></div>
              <span className="text-xs font-bold text-gray-400 bg-gray-50 px-2.5 py-1 rounded-md">{formatDate(log.date)}</span>
            </div>
          ))}
          {data.logs.length === 0 && <p className="text-gray-500 text-sm font-medium">Belum ada aktivitas tercatat.</p>}
        </div>
      </Card>
    </div>
  );
};

const GroupsPage = () => {
  const { data, performAction, showToast, currentUser } = useContext(DataContext);
  const [isModalOpen, setModalOpen] = useState(false);
  
  const [formData, setFormData] = useState({ 
    name: '', 
    method: 'Undi Berkala', 
    tenor: 'Bulanan', 
    nominal: '', 
    maxMembers: 10, 
    totalPeriods: 11 
  });

  const handleSubmit = async (e) => {
    e.preventDefault();
    const newGroup = { 
      id: generateId(), 
      tenantId: currentUser.tenantId, 
      name: formData.name, 
      method: formData.method, 
      tenor: formData.tenor,
      nominal: parseInt(formData.nominal), 
      maxMembers: parseInt(formData.maxMembers), 
      totalPeriods: parseInt(formData.totalPeriods), 
      status: 'Menunggu Anggota', 
      createdAt: new Date().toISOString() 
    };
    await performAction(() => api.insert('groups', newGroup), 'Buat Grup Baru', `Grup: ${newGroup.name}`);
    showToast('Grup berhasil dibuat');
    setModalOpen(false);
  };

  return (
    <div className="space-y-6">
      <div className="flex justify-between items-center">
        <h2 className="text-2xl font-black text-gray-900">Manajemen Grup</h2>
        <Button onClick={() => setModalOpen(true)} className="bg-emerald-600 hover:bg-emerald-700 border-none shadow-emerald-200 shadow-lg"><Plus size={18}/> Grup Baru</Button>
      </div>
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        {data.groups.map(g => {
          const members = data.groupMembers.filter(m => m.groupId === g.id);
          return (
            <Card key={g.id} className="relative hover:shadow-lg transition-shadow border-gray-200">
              <div className="absolute top-5 right-5"><Badge type={g.status === 'Aktif' ? 'success' : 'warning'}>{g.status}</Badge></div>
              <h3 className="font-black text-xl text-gray-900 mb-1">{g.name}</h3>
              <p className="text-sm text-gray-500 font-bold mb-6">{g.method} {g.tenor ? `• ${g.tenor}` : ''}</p>
              
              <div className="grid grid-cols-2 gap-4 text-sm mb-6 bg-gray-50 p-5 rounded-xl border border-gray-100">
                <div><p className="text-gray-500 mb-1 font-bold">Setoran</p><p className="font-black text-lg text-emerald-600">{formatRp(g.nominal)}</p></div>
                <div><p className="text-gray-500 mb-1 font-bold">Anggota</p><p className="font-black text-lg text-gray-900">{members.length} / {g.maxMembers}</p></div>
              </div>
            </Card>
          )
        })}
        {data.groups.length === 0 && <p className="text-gray-500 col-span-2 text-center py-12 bg-white rounded-2xl border border-dashed border-gray-300 font-bold">Belum ada grup arisan. Silakan buat grup pertama Anda.</p>}
      </div>
      <Modal isOpen={isModalOpen} onClose={() => setModalOpen(false)} title="Buat Grup Baru">
        <form onSubmit={handleSubmit} className="space-y-4">
          <Input label="Nama Grup" value={formData.name} onChange={e=>setFormData({...formData, name: e.target.value})} required />
          
          <div className="grid grid-cols-2 gap-4">
             <Select 
                label="Metode Arisan" 
                value={formData.method} 
                onChange={e=>setFormData({...formData, method: e.target.value})} 
                options={['Undi Berkala', 'Simpan Dana / Pencairan Akhir']} 
                required 
             />
             <Select 
                label="Tenor Setoran" 
                value={formData.tenor} 
                onChange={e=>setFormData({...formData, tenor: e.target.value})} 
                options={['Harian', 'Mingguan', 'Bulanan']} 
                required 
             />
          </div>

          <Input label="Nominal Setor (Rp)" type="number" value={formData.nominal} onChange={e=>setFormData({...formData, nominal: e.target.value})} required />
          <div className="grid grid-cols-2 gap-4">
            <Input label="Maks. Anggota" type="number" value={formData.maxMembers} onChange={e=>setFormData({...formData, maxMembers: e.target.value})} required />
            <Input label="Jml Periode" type="number" value={formData.totalPeriods} onChange={e=>setFormData({...formData, totalPeriods: e.target.value})} required />
          </div>
          <Button type="submit" className="w-full mt-4 bg-emerald-600 hover:bg-emerald-700 border-none text-white py-4">Simpan Grup</Button>
        </form>
      </Modal>
    </div>
  );
};

const MemberDashboard = () => {
  const { data, currentUser } = useContext(DataContext);
  return (
    <div className="space-y-6">
      <div className="bg-emerald-950 rounded-[2rem] p-8 text-white relative overflow-hidden shadow-2xl shadow-emerald-950/20">
         <div className="absolute top-0 right-0 w-64 h-64 bg-emerald-500/20 rounded-full blur-3xl translate-x-1/3 -translate-y-1/3"></div>
         <h2 className="text-3xl font-black relative z-10">Halo, {currentUser.name}!</h2>
         <p className="text-emerald-300 mt-2 relative z-10 font-medium">Ruang Arisan: <strong className="text-white">{data.tenant?.name || 'Arisan'}</strong></p>
      </div>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
         <Card>
           <h3 className="font-black mb-6 text-gray-900 text-lg">Grup Aktif Saya</h3>
           {data.groupMembers.filter(m=>m.userId === currentUser.id).length === 0 ? (
              <p className="text-sm text-gray-500 bg-gray-50 p-5 rounded-xl text-center border border-dashed border-gray-200 font-bold">Belum tergabung di grup arisan. Tunggu Admin memverifikasi & memasukkan Anda ke dalam grup.</p>
           ) : (
              data.groupMembers.filter(m=>m.userId === currentUser.id).map(m => {
                 const g = data.groups.find(group => group.id === m.groupId);
                 if(!g) return null;
                 return (
                   <div key={m.id} className="border-b border-gray-100 pb-4 mb-4 last:border-0 last:mb-0 last:pb-0">
                      <div className="flex justify-between items-center mb-2">
                         <p className="font-black text-gray-900 text-lg">{g.name}</p>
                         <Badge type="info">{g.method} {g.tenor ? `• ${g.tenor}` : ''}</Badge>
                      </div>
                      <p className="text-sm text-gray-500 font-bold">Tagihan Rutin: <span className="text-emerald-600 font-black">{formatRp(g.nominal)}</span></p>
                   </div>
                 )
               })
           )}
         </Card>
      </div>
    </div>
  );
};

const Sidebar = ({ role, currentView, setView, isMobileOpen, setMobileOpen }) => {
  const menu = role === 'admin' ? 
    [{ id: 'dashboard', icon: <Home size={20}/>, label: 'Dashboard' }, { id: 'groups', icon: <Layers size={20}/>, label: 'Grup Arisan' }] : 
    [{ id: 'dashboard', icon: <Home size={20}/>, label: 'Beranda' }];

  return (
    <>
      {isMobileOpen && <div className="fixed inset-0 bg-gray-900/60 z-40 lg:hidden backdrop-blur-sm" onClick={() => setMobileOpen(false)} />}
      <div className={`fixed inset-y-0 left-0 w-72 bg-emerald-950 text-emerald-100 z-50 transform transition-transform duration-300 ease-in-out lg:translate-x-0 ${isMobileOpen ? 'translate-x-0' : '-translate-x-full'}`}>
        <div className="p-8 border-b border-emerald-900 flex items-center gap-4">
           <div className="bg-emerald-500 p-2 rounded-xl text-emerald-950"><Layers size={24} strokeWidth={3} /></div>
           <h1 className="text-2xl font-black text-white tracking-tight">ArisanDigital</h1>
        </div>
        <div className="p-6 flex flex-col gap-3">
          {menu.map(item => (
            <button key={item.id} onClick={() => { setView(item.id); setMobileOpen(false); }} className={`w-full flex items-center gap-4 px-5 py-4 rounded-2xl transition-all font-bold ${currentView === item.id ? 'bg-emerald-900 text-white shadow-lg' : 'hover:bg-emerald-900 hover:text-white'}`}>
              {item.icon} {item.label}
            </button>
          ))}
        </div>
      </div>
    </>
  );
};

const Header = ({ setMobileOpen }) => {
  const { currentUser, logout } = useContext(DataContext);
  return (
    <header className="bg-white/80 backdrop-blur-md border-b border-gray-100 h-20 flex items-center justify-between px-6 lg:px-10 sticky top-0 z-30">
      <div className="flex items-center gap-4"><button className="lg:hidden p-2 text-gray-600 hover:bg-gray-100 rounded-xl transition-colors" onClick={() => setMobileOpen(true)}><Menu/></button></div>
      <div className="flex items-center gap-5">
        <div className="text-right hidden sm:block">
           <p className="text-sm font-black text-gray-900">{currentUser?.name}</p>
           <p className="text-xs text-gray-500 font-bold uppercase tracking-widest mt-0.5">{currentUser?.role}</p>
        </div>
        <div className="w-11 h-11 rounded-full bg-emerald-100 flex items-center justify-center text-emerald-700 font-black text-lg border-2 border-white shadow-md">{currentUser?.name?.charAt(0).toUpperCase()}</div>
        <button onClick={logout} className="text-gray-400 hover:text-red-500 p-2 transition-colors hover:bg-red-50 rounded-xl"><LogOut size={20}/></button>
      </div>
    </header>
  );
};

const MainLayout = () => {
  const { currentUser, isLoading } = useContext(DataContext);
  const [currentView, setCurrentView] = useState('dashboard');
  const [isMobileOpen, setMobileOpen] = useState(false);

  if(isLoading) return <div className="min-h-screen flex items-center justify-center bg-gray-50"><RefreshCw className="animate-spin text-emerald-500" size={40} strokeWidth={3}/></div>;

  return (
    <div className="min-h-screen bg-[#F8FAFC] flex text-gray-800 font-sans">
      <Sidebar role={currentUser.role} currentView={currentView} setView={setCurrentView} isMobileOpen={isMobileOpen} setMobileOpen={setMobileOpen} />
      <div className="flex-1 lg:ml-72 flex flex-col min-w-0">
        <Header setMobileOpen={setMobileOpen} />
        <main className="flex-1 p-6 lg:p-10 overflow-x-hidden">
          {currentUser.role === 'admin' ? (currentView === 'groups' ? <GroupsPage /> : <AdminDashboard />) : <MemberDashboard />}
        </main>
      </div>
    </div>
  );
};

export default function App() {
  return (
    <DataProvider>
      <DataContext.Consumer>
        {({ currentUser, isLoading }) => (
           isLoading ? <div className="min-h-screen flex items-center justify-center bg-gray-50"><RefreshCw className="animate-spin text-emerald-500" size={40} strokeWidth={3}/></div> :
           currentUser ? <MainLayout /> : <AuthPage />
        )}
      </DataContext.Consumer>
    </DataProvider>
  );
}
