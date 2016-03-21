Signature::

    * Calling convention: WINAPI
    * Category: misc


GetSystemMetrics
================

Signature::

    * Is success: ret != 0
    * Library: user32
    * Return value: int

Parameters::

    ** int nIndex index

Flags::

    index

Interesting::

    i index


GetCursorPos
============

Signature::

    * Library: user32
    * Return value: BOOL

Parameters::

    *  LPPOINT lpPoint

Logging::

    l x lpPoint != NULL ? lpPoint->x : 0
    l y lpPoint != NULL ? lpPoint->y : 0


GetComputerNameA
================

Signature::

    * Interesting: yes
    * Library: kernel32
    * Return value: BOOL

Parameters::

    *  LPCSTR lpBuffer
    *  LPDWORD lpnSize

Ensure::

    lpnSize

Logging::

    S computer_name *lpnSize, lpBuffer


GetComputerNameW
================

Signature::

    * Interesting: yes
    * Library: kernel32
    * Return value: BOOL

Parameters::

    *  LPWSTR lpBuffer
    *  LPDWORD lpnSize

Ensure::

    lpnSize

Logging::

    U computer_name *lpnSize / sizeof(wchar_t), lpBuffer


GetUserNameA
============

Signature::

    * Interesting: yes
    * Library: advapi32
    * Return value: BOOL

Parameters::

    *  LPCSTR lpBuffer
    *  LPDWORD lpnSize

Ensure::

    lpnSize

Logging::

    S user_name *lpnSize-1, lpBuffer


GetUserNameW
============

Signature::

    * Interesting: yes
    * Library: advapi32
    * Return value: BOOL

Parameters::

    *  LPWSTR lpBuffer
    *  LPDWORD lpnSize

Ensure::

    lpnSize

Logging::

    U user_name *lpnSize-1, lpBuffer


GetUserNameExA
==============

Signature::

    * Interesting: yes
    * Library: secur32
    * Return value: BOOL

Parameters::

    ** EXTENDED_NAME_FORMAT NameFormat name_format
    *  LPCSTR lpNameBuffer
    *  PULONG lpnSize

Ensure::

    lpnSize

Logging::

    S name *lpnSize, lpNameBuffer


GetUserNameExW
==============

Signature::

    * Interesting: yes
    * Library: secur32
    * Return value: BOOL

Parameters::

    ** EXTENDED_NAME_FORMAT NameFormat name_format
    *  LPWSTR lpNameBuffer
    *  PULONG lpnSize

Ensure::

    lpnSize

Logging::

    U name *lpnSize, lpNameBuffer


EnumWindows
===========

Signature::

    * Library: user32
    * Return value: BOOL

Parameters::

    *  WNDENUMPROC lpEnumProc
    *  LPARAM lParam


GetDiskFreeSpaceW
=================

Signature::

    * Interesting: yes
    * Library: kernel32
    * Return value: BOOL

Parameters::

    ** LPWSTR lpRootPathName root_path
    ** LPDWORD lpSectorsPerCluster sectors_per_cluster
    ** LPDWORD lpBytesPerSector bytes_per_sector
    ** LPDWORD lpNumberOfFreeClusters number_of_free_clusters
    ** LPDWORD lpTotalNumberOfClusters total_number_of_clusters


GetDiskFreeSpaceExW
===================

Signature::

    * Interesting: yes
    * Library: kernel32
    * Return value: BOOL

Parameters::

    ** LPWSTR lpDirectoryName root_path
    ** PULARGE_INTEGER lpFreeBytesAvailable free_bytes_available
    ** PULARGE_INTEGER lpTotalNumberOfBytes total_number_of_bytes
    ** PULARGE_INTEGER lpTotalNumberOfFreeBytes total_number_of_free_bytes


WriteConsoleA
=============

Signature::

    * Library: kernel32
    * Return value: BOOL

Parameters::

    ** HANDLE hConsoleOutput console_handle
    *  const VOID *lpBuffer
    *  DWORD nNumberOfCharsToWrite
    *  LPDWORD lpNumberOfCharsWritten
    *  LPVOID lpReseverd

Ensure::

    lpNumberOfCharsWritten

Logging::

    S buffer *lpNumberOfCharsWritten, lpBuffer


WriteConsoleW
=============

Signature::

    * Library: kernel32
    * Return value: BOOL

Parameters::

    ** HANDLE hConsoleOutput console_handle
    *  const VOID *lpBuffer
    *  DWORD nNumberOfCharsToWrite
    *  LPDWORD lpNumberOfCharsWritten
    *  LPVOID lpReseverd

Ensure::

    lpNumberOfCharsWritten

Logging::

    U buffer *lpNumberOfCharsWritten, lpBuffer


SHGetSpecialFolderLocation
==========================

Signature::

    * Library: shell32
    * Return value: HRESULT

Parameters::

    ** HWND hwndOwner window_handle
    ** int nFolder folder_index
    *  void *ppidl


SHGetFolderPathW
================

Signature::

    * Library: shell32
    * Return value: HRESULT

Parameters::

    ** HWND hwndOwner owner_handle
    ** int nFolder folder
    ** HANDLE hToken token_handle
    ** DWORD dwFlags flags
    *  LPWSTR pszPath

Flags::

    folder

Middle::

    wchar_t *dirpath = get_unicode_buffer();
    path_get_full_pathW(pszPath, dirpath);

Logging::

    u dirpath dirpath
    u dirpath_r pszPath

Post::

    free_unicode_buffer(dirpath);


LookupAccountSidW
=================

Signature::

    * Library: advapi32
    * Return value: BOOL

Parameters::

    ** LPCWSTR lpSystemName system_name
    *  PSID lpSid
    ** LPWSTR lpName account_name
    *  LPDWORD cchName
    ** LPWSTR lpReferencedDomainName domain_name
    *  LPDWORD cchReferencedDomainName
    *  PSID_NAME_USE peUse


ReadCabinetState
================

Signature::

    * Library: shell32
    * Return value: BOOL

Parameters::

    *  CABINETSTATE *pcs
    *  int cLength


UuidCreate
==========

Signature::

    * Is success: 1
    * Library: rpcrt4
    * Return value: RPC_STATUS

Parameters::

    *  UUID *Uuid

Middle::

    char uuid[128];
    clsid_to_string(Uuid, uuid);

Logging::

    s uuid uuid


GetTimeZoneInformation
======================

Signature::

    * Is success: ret != TIME_ZONE_ID_INVALID
    * Library: kernel32
    * Return value: DWORD

Parameters::

    *  LPTIME_ZONE_INFORMATION lpTimeZoneInformation


GetFileVersionInfoSizeW
=======================

Signature::

    * Is success: ret != 0
    * Library: version
    * Return value: DWORD

Parameters::

    ** LPCWSTR lptstrFilename filepath
    *  LPDWORD lpdwHandle


GetFileVersionInfoSizeExW
=========================

Signature::

    * Is success: ret != 0
    * Library: version
    * Prune: resolve
    * Return value: DWORD

Parameters::

    ** DWORD dwFlags flags
    ** LPCWSTR lptstrFilename filepath
    *  LPDWORD lpdwHandle


GetFileVersionInfoW
===================

Signature::

    * Library: version
    * Return value: BOOL

Parameters::

    ** LPCWSTR lptstrFilename filepath
    *  DWORD dwHandle
    *  DWORD dwLen
    *  LPVOID lpData

Logging::

    b buffer dwLen, lpData


GetFileVersionInfoExW
=====================

Signature::

    * Library: version
    * Prune: resolve
    * Return value: BOOL

Parameters::

    ** DWORD dwFlags flags
    ** LPCWSTR lptstrFilename filepath
    *  DWORD dwHandle
    *  DWORD dwLen
    *  LPVOID lpData

Logging::

    b buffer dwLen, lpData


NotifyBootConfigStatus
======================

Signature::

    * Library: advapi32
    * Return value: BOOL

Parameters::

    ** BOOL BootAcceptable boot_acceptable


TaskDialog
==========

Signature::

    * Library: comctl32
    * Prune: resolve
    * Return value: HRESULT

Parameters::

    ** HWND hWndParent parent_window_handle
    ** HINSTANCE hInstance instance_handle
    *  PCWSTR pszWindowTitle
    *  PCWSTR pszMainInstruction
    *  PCWSTR pszContent
    ** TASKDIALOG_COMMON_BUTTON_FLAGS dwCommonButtons buttons
    *  PCWSTR pszIcon
    ** int *pnButton button

Pre::

    wchar_t title_buf[10], description_buf[10], content_buf[10], icon_buf[10];
    wchar_t *title, *description, *content, *icon;

    int_or_strW(&title, pszWindowTitle, title_buf);
    int_or_strW(&description, pszMainInstruction, description_buf);
    int_or_strW(&content, pszContent, content_buf);
    int_or_strW(&icon, pszIcon, icon_buf);

Logging::

    u title title
    u description description
    u content content
    u icon icon


CreateActCtxW
=============

Signature::

    * Library: kernel32
    * Return value: HANDLE

Parameters::

    *  PACTCTX pActCtx

Logging::

    u resource_name pActCtx != NULL ? pActCtx->lpResourceName : NULL
    u application_name pActCtx != NULL ? pActCtx->lpApplicationName : NULL
    p module_handle pActCtx != NULL ? pActCtx->hModule : NULL

HeapCreate
==========

Signature::

    * Library: kernel32
    * Return value: HANDLE
    * Is success: ret != 0

Parameters::

    ** DWORD flOptions heap_options
    ** SIZE_T dwInitialSize heap_size
    ** SIZE_T dwMaximumSize heap_maxsize
