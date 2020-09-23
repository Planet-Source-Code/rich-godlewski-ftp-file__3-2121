<div align="center">

## ftp file


</div>

### Description

Copy a file from a LAN to UNIX and vice versa.
 
### More Info
 
Command line arguments (See code).

Set: Project/Settings/General to "Use MFC in a Static Library"

See code.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Rich Godlewski](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/rich-godlewski.md)
**Level**          |Intermediate
**User Rating**    |5.0 (15 globes from 3 users)
**Compatibility**  |Microsoft Visual C\+\+
**Category**       |[Files](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/files__3-2.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/rich-godlewski-ftp-file__3-2121/archive/master.zip)

### API Declarations

See code.


### Source Code

```
#include <afxinet.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>
// Set: Project/Settings/General to "Use MFC in a Static Library"
int main(int argc, char* argv[])
{
	char strFileType[1];
	char strDirection[1];
	if (argc < 8)
	{
		printf("\nUSAGE: ftpfile [LAN file] [UNIX file] [File Type: A=Ascii, B=Binary]");
		printf("\n[Direction: U=Copy to UNIX, L=Copy to LAN] [Server] [User ID] [Password]\n");
		return 0;
		exit(0);
	}
	CInternetSession iSession("ftpfile",1,INTERNET_OPEN_TYPE_DIRECT,NULL,NULL,0);
	CFtpConnection* FtpConn = iSession.GetFtpConnection(argv[5],argv[6],argv[7],INTERNET_INVALID_PORT_NUMBER,FALSE);
	// Check for Connection
	if(!FtpConn)
	{
		printf("\nERROR: Connection Failed. \nCheck the Server, User ID and Password.\n");
		return 0;
		exit(0);
	}
	// File Type Operations
	strncpy(strFileType,argv[3],2);
	strFileType[0]=toupper(strFileType[0]);
	if ((strcmp(strFileType, "A") !=0) && (strcmp(strFileType, "B") !=0))
	{
		printf("\nERROR: 'A' or 'B' Must be used for File Type.\n");
		FtpConn->Close();
		iSession.Close();
		delete iSession;
		return 0;
		exit(0);
	}
	// File Copy Operations
	strncpy(strDirection,argv[4],2);
	strDirection[0]=toupper(strDirection[0]);
	if ((strcmp(strDirection,"L") !=0) && (strcmp(strDirection,"U") !=0))
	{
		printf("\nERROR: 'L' or 'U' Must be used for Copy Direction.\n");
		FtpConn->Close();
		iSession.Close();
		delete iSession;
		return 0;
		exit(0);
	}
	// Upload to UNIX
	if (strcmp(strDirection, "U")==0)
	{
		if (strcmp(strFileType, "A")==0)
		{
			if (FtpConn->PutFile(argv[1], argv[2], FTP_TRANSFER_TYPE_ASCII, 0))
				printf("\nUpload Successfull\n");
			else
				printf("\nUpload Failed. \nCheck the file to be copied for existance.\nCheck the File Permissions.\n");
		}
		if (strcmp(strFileType, "B")==0)
		{
			if (FtpConn->PutFile(argv[1], argv[2], FTP_TRANSFER_TYPE_BINARY, 0))
				printf("\nUpload Successfull\n");
			else
				printf("\nUpload Failed. \nCheck the file to be copied for existance.\nCheck the File Permissions.");
		}
	}
	// Download to LAN
	if (strcmp(strDirection, "L")==0)
	{
		if (strcmp(strFileType, "A")==0)
		{
			if (FtpConn->GetFile(argv[2], argv[1], FALSE, FILE_ATTRIBUTE_NORMAL, FTP_TRANSFER_TYPE_ASCII, 0))
				printf("\nDownload Successfull\n");
			else
				printf("\nDownload Failed. \nCheck the file to be copied for existance.\nCheck the File Permissions.");
		}
		if (strcmp(strFileType, "B")==0)
		{
			if (FtpConn->GetFile(argv[2], argv[1], FALSE, FILE_ATTRIBUTE_NORMAL, FTP_TRANSFER_TYPE_BINARY, 0))
				printf("\nDownload Successfull\n");
			else
				printf("\nDownload Failed. \nCheck the file to be copied for existance.\nCheck the File Permissions.");
		}
	}
	// Close and Exit
	FtpConn->Close();
	iSession.Close();
	delete iSession;
	return 0;
	exit(0);
}
```

