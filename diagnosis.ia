import { useState, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { Textarea } from "@/components/ui/textarea";
import { Card, CardContent } from "@/components/ui/card";
import { Mic, Loader2, Stethoscope, Pill } from "lucide-react";
import { auth } from "@/lib/firebase";
import {
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword,
  onAuthStateChanged,
  signOut,
} from "firebase/auth";

export default function Home() {
  const [recording, setRecording] = useState(false);
  const [loading, setLoading] = useState(false);
  const [anamnese, setAnamnese] = useState("");
  const [diagnostico, setDiagnostico] = useState("");
  const [prescricao, setPrescricao] = useState("");
  const [user, setUser] = useState(null);
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  useEffect(() => {
    const unsubscribe = onAuthStateChanged(auth, (currentUser) => {
      setUser(currentUser);
    });
    return () => unsubscribe();
  }, []);

  const handleLogin = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password);
    } catch (error) {
      alert("Erro ao fazer login: " + error.message);
    }
  };

  const handleRegister = async () => {
    try {
      await createUserWithEmailAndPassword(auth, email, password);
    } catch (error) {
      alert("Erro ao registrar: " + error.message);
    }
  };

  const handleLogout = async () => {
    await signOut(auth);
  };

  const iniciarGravacao = async () => {
    setRecording(true);
    setTimeout(() => {
      setRecording(false);
      setAnamnese("Paciente masculino, 45 anos, com dor torácica súbita há 2 horas, irradiando para braço esquerdo, sudorese, náuseas. PA 150x90, FC 100 bpm.");
    }, 3000);
  };

  const sugerirDiagnostico = async () => {
    setLoading(true);
    setDiagnostico("");
    setPrescricao("");
    setTimeout(() => {
      setDiagnostico("🔎 Hipóteses Diagnósticas:\n- Síndrome coronariana aguda\n- Angina instável\n- Infarto agudo do miocárdio\n\n🧪 Sugestão de exames:\n- ECG\n- Troponina\n- Hemograma completo\n- RX de tórax\n- Glicemia e perfil lipídico");
      setLoading(false);
    }, 2000);
  };

  const sugerirPrescricao = async () => {
    setLoading(true);
    setTimeout(() => {
      setPrescricao("💊 Prescrição sugerida:\n- Ácido acetilsalicílico 300 mg VO, dose única\n- Clopidogrel 300 mg VO, dose única\n- Enoxaparina 1 mg/kg SC, 12/12h\n- Atenolol 50 mg VO, 1x/dia\n- Monitorar ECG e enzimas cardíacas\n\n⚠️ Confirmar conduta com protocolo local e perfil do paciente.");
      setLoading(false);
    }, 2000);
  };

  if (!user) {
    return (
      <main className="p-4 max-w-md mx-auto space-y-4">
        <h1 className="text-2xl font-bold text-center">MediAssist AI</h1>
        <Card>
          <CardContent className="space-y-4 p-4">
            <input
              type="email"
              placeholder="Email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              className="w-full p-2 border rounded"
            />
            <input
              type="password"
              placeholder="Senha"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              className="w-full p-2 border rounded"
            />
            <Button className="w-full" onClick={handleLogin}>Entrar</Button>
            <Button className="w-full" variant="outline" onClick={handleRegister}>Cadastrar</Button>
          </CardContent>
        </Card>
      </main>
    );
  }

  return (
    <main className="p-4 max-w-2xl mx-auto space-y-4">
      <div className="flex justify-between items-center">
        <h1 className="text-2xl font-bold">MediAssist AI</h1>
        <Button variant="outline" onClick={handleLogout}>Sair</Button>
      </div>

      <Card>
        <CardContent className="space-y-4 p-4">
          <Button onClick={iniciarGravacao} disabled={recording} className="w-full justify-center">
            {recording ? <Loader2 className="animate-spin mr-2" /> : <Mic className="mr-2" />} Gravar Anamnese
          </Button>

          <Textarea
            placeholder="Anamnese aparecerá aqui..."
            value={anamnese}
            onChange={(e) => setAnamnese(e.target.value)}
          />

          <Button onClick={sugerirDiagnostico} disabled={loading || !anamnese} className="w-full justify-center">
            {loading ? <Loader2 className="animate-spin mr-2" /> : <Stethoscope className="mr-2" />} Sugerir Diagnóstico
          </Button>

          {diagnostico && (
            <div className="p-2 bg-blue-100 rounded text-blue-800 whitespace-pre-wrap">
              {diagnostico}
            </div>
          )}

          <Button onClick={sugerirPrescricao} disabled={loading || !diagnostico} className="w-full justify-center">
            {loading ? <Loader2 className="animate-spin mr-2" /> : <Pill className="mr-2" />} Sugerir Prescrição
          </Button>

          {prescricao && (
            <div className="p-2 bg-green-100 rounded text-green-800 whitespace-pre-wrap">
              {prescricao}
            </div>
          )}
        </CardContent>
      </Card>
    </main>
  );
}
