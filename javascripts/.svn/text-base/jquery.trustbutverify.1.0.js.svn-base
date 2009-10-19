(function($) {
  $.tbv = { };
  $.extend($.tbv, {
    settings: {
      inited: false,
      wait: 500,
      errorWait: 500,
      validMessage: '  ',
      friendlyAttr: 'friendly'
    },
    validators:[],
    getValidator: function(id) {
      if(this.validators[id] == undefined) {
        this.validators[id] = new LiveValidation( id, { validMessage: this.settings.validMessage, wait: $.tbv.settings.wait });
      }
      return this.validators[id];
    },
    setup: function(e, type, defaultMessage, callback, message, additionalArgs) {
      if(!$.tbv.settings.inited) init();
      $(e).each(function(i,e){
        if($(e).attr('id') == '') return; //ignore elements with an ID
        var msg, 
            val = $.tbv.getValidator($(e).attr('id'));
        if(message == undefined && defaultMessage != undefined) {
          var name = $(e).attr($.tbv.settings.friendlyAttr);
          if(name == undefined) name = $(e).attr('id');
          msg = name + defaultMessage;
        } else { msg = message; }
        var standardArgs = { failureMessage: msg, wait: $.tbv.settings.errorWait }
        if(type == Validate.Custom) { // cannot just pass additionalArgs because we want the current $(e) in the callback
          val.add( Validate.Custom, $.extend(standardArgs, { against: function(v,args){ return callback(v, args.element); }, args: { element: $(e) } }));
        }
        else if(type == Validate.Presence) {
          val.add( Validate.Presence, standardArgs);
        }
        else if(type == Validate.Email) {
          val.add( Validate.Email, standardArgs);
        }
        else if(type == Validate.Length) {
          val.add( Validate.Length, $.extend(standardArgs, $.extend({ wrongLengthMessage: msg, tooShortMessage: msg, tooLongMessage: msg }, additionalArgs)));
        }
        else if(type == Validate.Format) {
          val.add( Validate.Format, $.extend(standardArgs, additionalArgs) );
        }
      });  
      return e;  
    } 
  });
  
  $.fn.required = function(message) {
    return $.tbv.setup(this, Validate.Presence, ' is required', undefined, message);
  }
  
  $.fn.validate = function(callback, message) {
    return $.tbv.setup(this, Validate.Custom, ' is invalid', callback, message);
  }
  
  $.fn.matches = function(regex, message) {
    return $.tbv.setup(this, Validate.Format, ' is invalid', undefined, message, { pattern: regex });
  }
  
  $.fn.doesntMatch = function(regex, message) {
    return $.tbv.setup(this, Validate.Format, ' is invalid', undefined, message, { pattern: regex, negate: true });
  }
  
  $.fn.validEmail = function(message) {
    return $.tbv.setup(this, Validate.Email, ' is not a valid email', undefined, message);
  }
  
  $.fn.validLength = function(additionalArgs, message) {
    return $.tbv.setup(this, Validate.Length, undefined, undefined, message, additionalArgs);
  }
    
  function init(settings) {
    if ($.tbv.settings.inited) return true
    else $.tbv.settings.inited = true
    if (settings) $.extend($.tbv.settings, settings)
  }
})(jQuery);
