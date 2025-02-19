```markdown
### Core
[ ] 1.1 Create base Next.js app with TypeScript  
**Validation**: `npm run dev` starts without errors and shows starter page  
[ ] 1.2 Setup core directories (pages/api, components, lib)  
**Validation**: Basic API route responds at /api/test  
[ ] 1.3 Install core dependencies (neynar, openai, react-use)  
**Validation**: Import statements work in sample component  
[ ] 6.1 Implement Zustand store with basic state shape  
**Validation**: Console.log shows initial state in component  
[ ] 6.2 Sync state with URL params using base64  
**Validation**: State persists through page reload  
[ ] 6.3 Add local storage backup/restore  
**Validation**: State recovers after browser restart  
[ ] 10.3 Configure production build optimizations  
**Validation**: `npm run build` completes with no errors  

### API
[ ] 3.1 Create /api/process route skeleton  
**Validation**: POST request returns empty 200 response  
[ ] 3.2 Implement Zod validation schema  
**Validation**: Invalid requests return 400 error  
[ ] 3.3 Add rate limiter middleware  
**Validation**: Multiple rapid requests trigger 429  
[ ] 4.1 Initialize Neynar client with env vars  
**Validation**: API key validated in test endpoint  
[ ] 4.2 Implement docs fetcher with retry logic  
**Validation**: Failed requests retry 3 times in console  
[ ] 5.1 Create LLM stage skeleton with timeouts  
**Validation**: Empty processing chain returns mock data  

### UI
[ ] 2.1 Build InputForm with character counter  
**Validation**: Input shows 100-1000 char validation  
[ ] 2.2 Implement sanitization feedback  
**Validation**: Special characters show warning  
[ ] 7.1 Create collapsible Sidebar component  
**Validation**: Toggle button works in desktop/mobile  
[ ] 7.2 Add process timeline with status badges  
**Validation**: Mock stages show completion states  
[ ] 8.1 Integrate Monaco code editor  
**Validation**: Sample code displays with syntax highlighting  
[ ] 9.1 Create loading spinner component  
**Validation**: Spinner appears during mock API call  
```

**Implementation Notes**  
1. Core foundations first (project setup > state > optimizations)  
2. API routes before dependent UI components  
3. UI components ordered by user flow priority  
4. Validation criteria verify functionality without full integration  
5. Later stages build on validated foundation from earlier tasks