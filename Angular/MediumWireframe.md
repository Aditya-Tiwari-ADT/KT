## Pages

- Homepage — paginated article listing with filtering
- Search — query, filters, results
- dashboard — user's articles (paginated); create / edit / delete
- Article page — display single article
- 404 / error boundary

## Modules

- auth- auth components,routing,lazy loading
- core-auth--auth service,auth guards,articles service
- shared--Navbar,404,error boundry component,card,quill editor and viewer,filters,search component,logo,profile

## Services

- auth--auth service-login,register,logout,isAuthenticated,getCurrentUser,httpInterceptors,
- articles--fetch ,add ,update ,delete (CRUD service)
  --get articles based on filtering,get articles of loggedin user,search articles (seraching and filtering service)

Note- We are not keeping search as a seperate module because in our app seraching is only related to articles so we can keep it there

## 1. Authentication

### Login

- public, guarded
- App logo
- Email input
- Password input
- Login button
- Register link

### Registration

- public, guarded
- App logo
- Email input
- Password input
- Confirm password input
- Register button
- Link to login

## 2. Admin Dashboard (Articles CRUD)

- Protected
- Navbar: Logo | Home | Dashboard | Logout
- Create Article button
- Article list (AGgrid)
  - Columns: Title, Author, Last modified, Tags, Actions (edit/delete)
  - Pagination controls

## 3. Article Page

- Protected
- Article title
- Author name
- Last modified date
- Article image
- Content (rendered Quill)

## 4. Homepage (Listings)

- Navigation- Home,dashboard,login/logout
- Filters
- Search bar
- Article cards in a list or grid: Grid component(shared) and card component(In articles module)
- Pagination controls

## 5. Search Page

- Public
- Navbar: Logo | Home | Search | Profile | Logout
- Search input
- Results grid / list:
  - Item: Title, Description, Tags, Author

## 6. 404 Error Page

- App logo
- 404 message ("Page not found")
- Button/link to homepage
