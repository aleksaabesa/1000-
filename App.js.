import React, { useState, useEffect } from 'react';
import { View, Text, Button, Image, TextInput, ScrollView } from 'react-native';
import { BarCodeScanner } from 'expo-barcode-scanner';
import QRCode from 'react-native-qrcode-svg';

// მთავარი კომპონენტი
export default function App() {
  const [user, setUser] = useState(null); // სისტემაში შესული მომხმარებელი
  const [email, setEmail] = useState('');
  const [isAdmin, setIsAdmin] = useState(false);
  
  // ადმინის შემოწმება შენი მოთხოვნის მიხედვით
  const handleLogin = () => {
    if (email === '100@gmail.com') {
      setIsAdmin(true);
    } else {
      setIsAdmin(false);
    }
    setUser(email);
  };

  if (!user) {
    return (
      <View style={{ flex: 1, justifyContent: 'center', padding: 20 }}>
        <Text style={{ fontSize: 24, marginBottom: 20 }}>ავტორიზაცია: 1000 ნივთი</Text>
        <TextInput 
          placeholder="შეიყვანეთ იმეილი" 
          onChangeText={setEmail}
          style={{ borderBottomWidth: 1, marginBottom: 20 }}
        />
        <Button title="შესვლა" onPress={handleLogin} />
      </View>
    );
  }

  return (
    <View style={{ flex: 1 }}>
      {isAdmin ? <AdminPanel /> : <UserPanel userEmail={user} />}
    </View>
  );
}

// --- ადმინის პანელი ---
function AdminPanel() {
  const [hasPermission, setHasPermission] = useState(null);
  const [scannedData, setScannedData] = useState(null);
  const [isScanning, setIsScanning] = useState(false);

  useEffect(() => {
    (async () => {
      const { status } = await BarCodeScanner.requestPermissionsAsync();
      setHasPermission(status === 'granted');
    })();
  }, []);

  const handleBarCodeScanned = ({ data }) => {
    setIsScanning(false);
    // მონაცემების წაკითხვა (ფორმატი: "პროდუქტი|იმეილი")
    setScannedData(data);
    alert(`სკანირებულია! \nინფორმაცია: ${data} \nსტატუსი: ნაყიდია`);
  };

  return (
    <ScrollView style={{ padding: 20, marginTop: 40 }}>
      <Text style={{ fontSize: 22, fontWeight: 'bold' }}>ადმინ პანელი</Text>
      
      <View style={{ marginVertical: 20, padding: 15, backgroundColor: '#f0f0f0' }}>
        <Text>ნივთის დამატება (ფოტო/ფასი/სახელი)</Text>
        <TextInput placeholder="ნივთის სახელი" style={inputStyle} />
        <TextInput placeholder="ფასი" keyboardType="numeric" style={inputStyle} />
        <Button title="ფოტოს გადაღება/ატვირთვა" onPress={() => {}} />
        <Button title="ბაზაში დამატება" color="green" onPress={() => alert('დაემატა!')} />
      </View>

      <Button title="სკანერის ჩართვა" onPress={() => setIsScanning(true)} />
      
      {isScanning && (
        <BarCodeScanner
          onBarCodeScanned={handleBarCodeScanned}
          style={{ height: 300, width: '100%' }}
        />
      )}
    </ScrollView>
  );
}

// --- მომხმარებლის პანელი ---
function UserPanel({ userEmail }) {
  const [productBought, setProductBought] = useState(null);

  const buyProduct = () => {
    setProductBought({ name: "მაგიდა", price: "150₾" });
  };

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center', padding: 20 }}>
      <Text>მოგესალმებით: {userEmail}</Text>
      {!productBought ? (
        <View>
          <Text style={{ fontSize: 18, marginVertical: 10 }}>ნივთი: მაგიდა - 150₾</Text>
          <Button title="ყიდვა" onPress={buyProduct} />
        </View>
      ) : (
        <View style={{ alignItems: 'center' }}>
          <Text style={{ color: 'green', marginBottom: 10 }}>თქვენ წარმატებით იყიდეთ ნივთი!</Text>
          <QRCode value={`პროდუქტი: ${productBought.name} | მყიდველი: ${userEmail}`} size={200} />
          <Text style={{ marginTop: 10 }}>წარუდგინეთ ეს კოდი ადმინს</Text>
        </View>
      )}
    </View>
  );
}

const inputStyle = { borderBottomWidth: 1, marginBottom: 10, padding: 5 };
