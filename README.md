# Roblox-Disable-Logs
stops roblox from uploading your logs and reduces chances of getting banned

```C++
#define log_addy_start 0xB838D
 int WINAPI h_MessageBox(
	 _In_opt_ HWND    hWnd,
	 _In_opt_ LPCTSTR lpText,
	 _In_opt_ LPCTSTR lpCaption,
	 _In_     UINT    uType
	 ){
	 if (lpCaption == "Roblox Crash"){
		 DWORD log_start = log_addy_start + (DWORD)GetModuleHandleA(NULL);
		 DWORD old;
		 for (int i = 0; i < 79; i++){
			 VirtualProtect((LPVOID)(log_start + i), 1, PAGE_EXECUTE_READWRITE, &old);
			 *(char*)(log_start + i) = 0x90;
			 VirtualProtect((LPVOID)(log_start + i), 1, old, &old);
		 }
		 lpText = "An unexpected error occurred and Roblox needs to quit. (logs wont get uploaded)";
	 }
	 return MessageBoxW(hWnd, (LPCWSTR)lpText, (LPCWSTR)lpCaption, uType);
	}

 void InitUploadCheckHook(){
	 DWORD o;
	 VirtualProtect((LPVOID)&MessageBoxA, 0x05, PAGE_EXECUTE_READWRITE, &o);
	 *(char*)(&MessageBoxA) = 0xE9;
	 *(DWORD*)((DWORD)&MessageBoxA + 1) = ((DWORD)&h_MessageBox - (DWORD)&MessageBoxA) - 5;
	 VirtualProtect((LPVOID)&MessageBoxA, 0x05, o, &o);
 }


```
