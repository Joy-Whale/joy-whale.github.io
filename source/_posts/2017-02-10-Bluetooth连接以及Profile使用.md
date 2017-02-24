layout: android
title: Bluetooth连接以及Profile使用
date: 2017-02-10 15:56:25
categories:
- 技术
- Android
tags: 
- Android
- Bluetooth
---
## 前言
 　随着智能设备的普及，蓝牙开发在手机应用端使用的越来越多。但是目前网上对蓝牙开发的介绍还相当少，刚开始接触蓝牙开发时，都是边网上搜边看源码，花费了不少功夫，总算是对蓝牙开发略知一二了。

## 解析
   蓝牙设备之间建立连接分为三个部分：1.Bond（Pair）、2.Profile、3.Socket

### 1.Bond
&#8195;&#8195;Bond即设备之间绑定（配对），这是蓝牙设备之间通信的基础。当搜索到需要bond的设备时，获取到设备对应的BluetoothDevice对象时，我们就可以进行bond操作了。查看BluetoothDevice源码，在其方法中可以找到一个叫“createBond()”方法，该方法即是蓝牙bond的核心方法，但需要注意的是，该方法在API19之前是@hide的，所以minSdk设置小于API19时，需要通过反射来调用该方法

```
 /**
     * Start the bonding (pairing) process with the remote device.
     * <p>This is an asynchronous call, it will return immediately. Register
     * for {@link #ACTION_BOND_STATE_CHANGED} intents to be notified when
     * the bonding process completes, and its result.
     * <p>Android system services will handle the necessary user interactions
     * to confirm and complete the bonding process.
     * <p>Requires {@link android.Manifest.permission#BLUETOOTH_ADMIN}.
     *
     * @return false on immediate error, true if bonding will begin
     */
    @RequiresPermission(Manifest.permission.BLUETOOTH_ADMIN)
    public boolean createBond() {
        if (sService == null) {
            Log.e(TAG, "BT not enabled. Cannot create bond to Remote Device");
            return false;
        }
        try {
            return sService.createBond(this, TRANSPORT_AUTO);
        } catch (RemoteException e) {Log.e(TAG, "", e);}
        return false;
    }
```
通过查看源码，不难看出，createBond方法在执行时，调用了IPC，需要了解具体实现过程的，可以去查看“sService ”这个IPC类的源码，这里就不再深入。

### 2.Profile
&#8195;&#8195;Profile是蓝牙的一个很重要特性，Profile定义了设备如何实现一种连接或者应用，你可以把Profile理解为连接层或者应用层协。

下面来介绍Profile的连接：
　在android framework层中，Profile同样是封装成了一个个IPC类，在BluetoothAdapter中提供了"getProfileProxy(Context context, BluetoothProfile.ServiceListener listener, int profile)"方法连接IPC实例获取到这些Profile服务的代理来操作这些profile以及"closeProfileProxy(int profile, BluetoothProfile profile)"来关闭这些Profile的代理

```
/**
     * Get the profile proxy object associated with the profile.
     *
     * <p>Profile can be one of {@link BluetoothProfile#HEALTH}, {@link BluetoothProfile#HEADSET},
     * {@link BluetoothProfile#A2DP}, {@link BluetoothProfile#GATT}, or
     * {@link BluetoothProfile#GATT_SERVER}. Clients must implement
     * {@link BluetoothProfile.ServiceListener} to get notified of
     * the connection status and to get the proxy object.
     *
     * @param context Context of the application
     * @param listener The service Listener for connection callbacks.
     * @param profile The Bluetooth profile; either {@link BluetoothProfile#HEALTH},
     *                {@link BluetoothProfile#HEADSET}, {@link BluetoothProfile#A2DP}.
     *                {@link BluetoothProfile#GATT} or {@link BluetoothProfile#GATT_SERVER}.
     * @return true on success, false on error
     */
    public boolean getProfileProxy(Context context, BluetoothProfile.ServiceListener listener,
                                   int profile) {
        if (context == null || listener == null) return false;
 
        if (profile == BluetoothProfile.HEADSET) {
            BluetoothHeadset headset = new BluetoothHeadset(context, listener);
            return true;
        } else if (profile == BluetoothProfile.A2DP) {
            BluetoothA2dp a2dp = new BluetoothA2dp(context, listener);
            return true;
        } else if (profile == BluetoothProfile.A2DP_SINK) {
            BluetoothA2dpSink a2dpSink = new BluetoothA2dpSink(context, listener);
            return true;
        } else if (profile == BluetoothProfile.AVRCP_CONTROLLER) {
            BluetoothAvrcpController avrcp = new BluetoothAvrcpController(context, listener);
            return true;
        } else if (profile == BluetoothProfile.INPUT_DEVICE) {
            BluetoothInputDevice iDev = new BluetoothInputDevice(context, listener);
            return true;
        } else if (profile == BluetoothProfile.PAN) {
            BluetoothPan pan = new BluetoothPan(context, listener);
            return true;
        } else if (profile == BluetoothProfile.HEALTH) {
            BluetoothHealth health = new BluetoothHealth(context, listener);
            return true;
        } else if (profile == BluetoothProfile.MAP) {
            BluetoothMap map = new BluetoothMap(context, listener);
            return true;
        } else if (profile == BluetoothProfile.HEADSET_CLIENT) {
            BluetoothHeadsetClient headsetClient = new BluetoothHeadsetClient(context, listener);
            return true;
        } else if (profile == BluetoothProfile.SAP) {
            BluetoothSap sap = new BluetoothSap(context, listener);
            return true;
        } else {
            return false;
        }
    }
```
　该方法支持连接HEADSET、A2DP、A2DP_SINK、AVRCP、INPUT_DEVICE、PAN、HEALTH、MAP、HEADSET_CLIENT、SAP等相应的Profile。
　
　参数中，BluetoothProfile.SearchListener是对外用来监听是否成功连接Profile的IPC实例的，与普通AIDL用来监听连接的ServiceConnection类似，当“onServiceConnected(int profile, BluetoothProfile proxy)”被调用时，我们就可以获取到该Profile的IPC实例来做我们想做的事情。
    
　这里已Headset来讲解Profile连接设备的方式，其实上述这些Profile的连接方式都大同小异。在上面方法中，当获取HEADSET Profile时，生成了一个BluetoothHeadset对象，该对象即是Headset IPC的代理，在该类中有"connect(BluetoothDevice device)"、"disconnect(BluetoothDevice device)"方法来连接和断开与设备的Profile链接等。
```
/**
     * Initiate connection to a profile of the remote bluetooth device.
     *
     * <p> Currently, the system supports only 1 connection to the
     * headset/handsfree profile. The API will automatically disconnect connected
     * devices before connecting.
     *
     * <p> This API returns false in scenarios like the profile on the
     * device is already connected or Bluetooth is not turned on.
     * When this API returns true, it is guaranteed that
     * connection state intent for the profile will be broadcasted with
     * the state. Users can get the connection state of the profile
     * from this intent.
     *
     * <p>Requires {@link android.Manifest.permission#BLUETOOTH_ADMIN}
     * permission.
     *
     * @param device Remote Bluetooth Device
     * @return false on immediate error,
     *               true otherwise
     * @hide
     */
    public boolean connect(BluetoothDevice device) {
        if (DBG) log("connect(" + device + ")");
        if (mService != null && isEnabled() &&
            isValidDevice(device)) {
            try {
                return mService.connect(device);
            } catch (RemoteException e) {
                Log.e(TAG, Log.getStackTraceString(new Throwable()));
                return false;
            }
        }
        if (mService == null) Log.w(TAG, "Proxy not attached to service");
        return false;
    }
 
    /**
     * Initiate disconnection from a profile
     *
     * <p> This API will return false in scenarios like the profile on the
     * Bluetooth device is not in connected state etc. When this API returns,
     * true, it is guaranteed that the connection state change
     * intent will be broadcasted with the state. Users can get the
     * disconnection state of the profile from this intent.
     *
     * <p> If the disconnection is initiated by a remote device, the state
     * will transition from {@link #STATE_CONNECTED} to
     * {@link #STATE_DISCONNECTED}. If the disconnect is initiated by the
     * host (local) device the state will transition from
     * {@link #STATE_CONNECTED} to state {@link #STATE_DISCONNECTING} to
     * state {@link #STATE_DISCONNECTED}. The transition to
     * {@link #STATE_DISCONNECTING} can be used to distinguish between the
     * two scenarios.
     *
     * <p>Requires {@link android.Manifest.permission#BLUETOOTH_ADMIN}
     * permission.
     *
     * @param device Remote Bluetooth Device
     * @return false on immediate error,
     *               true otherwise
     * @hide
     */
    public boolean disconnect(BluetoothDevice device) {
        if (DBG) log("disconnect(" + device + ")");
        if (mService != null && isEnabled() &&
            isValidDevice(device)) {
            try {
                return mService.disconnect(device);
            } catch (RemoteException e) {
              Log.e(TAG, Log.getStackTraceString(new Throwable()));
              return false;
            }
        }
        if (mService == null) Log.w(TAG, "Proxy not attached to service");
        return false;
    }
```
源码中都有非常详细的方法介绍，这里就不再敖诉。

### 3.Socket
&#8195;&#8195;绑定后的蓝牙设备之间是可以建立Socket通信的，这种Socket类似于TCP Socket，但略有不同，该Socket只能通过调用Android API来获取并连接，但通信操作是与TCP相同的，可以获取InputStream以及OutputStream来实现数据的交互。在蓝牙规范中，有个SPP(全称Serial Port Profile) Profile，定义了如何在两台蓝牙设备之间建立虚拟串口并进行连接，顾名思义，就是来支持蓝牙设备之间的Socket通信。该Profile在蓝牙设备绑定之后便会连接，所以我们只需在蓝牙绑定成功后，通过调用相应的API，即可获取BluetoothSocket对象，在该对象中提供了"getInputStream"、"getOutputStream"来获取输入输出流，然后即可通过IO流来传输数据，实现设备通讯。
 
　一般的，我们通过连接该Socket来实现手机控制智能设备的功能，但是Socket也仅仅只能用来数据通讯，要想实现免提电话、听音乐、外接键盘等功能，还是需要去连接对应的Profile。

下面附上自己做的一个小Demo：https://github.com/Joy-Whale/BluetoothProfile