# Authenticate agains AD

Active directory is everywhere. Since many of the organizations already use it, it is often convenient to use AD to authenticate users.

Here is an example of how to do that:

    @Service
    public class ActiveDirectoryService {

	@Value("${adServer}")
	String adServer;

	@Value("${authentication}")
	String authentication;

	@Value("${ldapSearchBase}")
	String ldapSearchBase;

	@Value("${group}")
	String group;

    public boolean authenticate(User user) throws NamingException {
    String ldapUsername = user.getUsername();
    String ldapPassword = String.valueOf(user.getPassword());

    try{
    Hashtable<String, Object> env = new Hashtable<>();
    env.put(Context.SECURITY_AUTHENTICATION, authentication);
    env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
    env.put(Context.PROVIDER_URL, adServer);
    env.put("java.naming.ldap.attributes.binary", "objectSID");

    if(ldapUsername != null) env.put(Context.SECURITY_PRINCIPAL, ldapUsername);
    if(ldapPassword != null) env.put(Context.SECURITY_CREDENTIALS, ldapPassword);

    SearchResult srLdapUser = findAccountByAccountName(new InitialDirContext(env), ldapSearchBase, ldapUsername);

    return new InitialDirContext(env) != null
     &&  srLdapUser.getAttributes().get("memberof").toString().contains(group);
    } catch (Exception e) { e.printStackTrace(); }
		return false;
    }


	public SearchResult findAccountByAccountName(DirContext ctx, String ldapSearchBase, String accountName) throws NamingException {

	 String searchFilter="(&(objectClass=user)(sAMAccountName="+accountName+"))";

	 SearchControls searchControls = new SearchControls();
	 searchControls.setSearchScope(SearchControls.SUBTREE_SCOPE);

	 NamingEnumeration<SearchResult> results = ctx.search(ldapSearchBase, searchFilter, searchControls);

	 SearchResult searchResult = null;
	 if (results.hasMoreElements()) {
		searchResult = (SearchResult) results.nextElement();
		if (results.hasMoreElements()) return null;
	 }
	 return searchResult;
	 }
    }

If ldap context binds as that user, we know that the username and password are valid. Additionally, we can check if the user belongs to a certain group.
