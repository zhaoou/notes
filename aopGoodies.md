# AOP goodies

AOP is great.

Today, I configured an authentication enforcement and simple audit for one of the internal applications, using spring AOP support.

As it turns out, it is simpler than I expected.

If your log-in/out logic is located in a separate controller(as it should) then defining appropriate pointcuts allows us to let anybody access actions in the login controller, and enforce logged in users everywhere else. I understand that this is not the same level of security as spring security provides, but I think (and my manager, who is a big security guy, agrees) is sufficient for many applications, assuming all other aspects of security are taken care of.

    @Aspect
    @Component
    public class AuditAspect {

	@Autowired
	HttpSession session;

	    @Pointcut("within(@org.springframework.stereotype.Controller *)")
	    public void controller() {}

	    @Pointcut("execution(* *(..))")
	    public void method() {}

	    @Pointcut("execution(* com.s.controller.LoginC.*(..))")
	    public void login() {}

	    @AfterReturning("controller() && method() ")
	    public void after(JoinPoint jp) {
	    	if(session.getAttribute("producer") != null){
    		    System.out.println(jp.toLongString());
	    	}
	    }

	    @Around("controller() && method() && not login()")
	    public Object around(ProceedingJoinPoint jp) {
	    	try {
			    if(session.getAttribute("user") != null){
			    	System.out.println("proceeding..");
			    	return jp.proceed();
			    } else {
			    	System.out.println("not proceeding..");
			    	return "redirect:/login_form";
			    }
		    } catch (Throwable e) {
		    	System.out.println("error");
		    }
	    	return "redirect:/login_form";
	    }
    }


* **after** method here can used for logging of who does what: we can get user currently signed in from the session, and make sure that our action method have descriptive names.

* **around** method effectively restricts access to actions outside of LoginController to signed in users only. All methods in LoginController are not included into the pointcut, and hence have unrestricted access.

Stay tuned for more dumps!
