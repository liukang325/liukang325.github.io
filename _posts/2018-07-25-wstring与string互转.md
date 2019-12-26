---
layout: post
title: wstring 与 string 互转
category: C++
tags: C++ Windows
keywords: 
description: 
---

此博客均属原创或译文，欢迎转载但**请注明出处** 
**GithubPage:**[https://liukang325.github.io](https://liukang325.github.io)

```c++
static std::wstring StringToUnicode(const std::string& strStr, UINT codePage = CP_UTF8)
{
	std::wstring retStr;
	int nLen = MultiByteToWideChar(codePage, 0, strStr.c_str(), -1, NULL, 0);
	if (nLen != 0)
	{
		wchar_t* pResult = new wchar_t[nLen];
		MultiByteToWideChar(codePage, 0, strStr.c_str(), -1, pResult, nLen);
		retStr = pResult;
		delete[] pResult;
	}

	return retStr;
}
static std::string UnicodeToString(const std::wstring& strWstr, UINT codePage = CP_UTF8)
{
	std::string retStr;
	int nLen = WideCharToMultiByte(codePage, 0, strWstr.c_str(), -1, NULL, 0, NULL, NULL);
	if (nLen != 0)
	{
		char* pResult = new char[nLen];
		WideCharToMultiByte(codePage, 0, strWstr.c_str(), -1, pResult, nLen, NULL, NULL);

		retStr = pResult;
		delete[] pResult;
	}
	return retStr;
}
#define s2ws(s) StringToUnicode(s)
#define ws2s(s) UnicodeToString(s)
```